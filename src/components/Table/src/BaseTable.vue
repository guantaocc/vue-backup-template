<template>
  <div class="base-table">
    <div class="table-header">
      <slot name="header">
        <TableHeader :table-columns="tableColumns" @redo="redo" @table-size-change="handleTableSizeChange" @table-column-change="handleColumnChange">
          <template #tableTitle>
            <slot name="tableTitle" />
          </template>
          <template #toolbar>
            <slot name="toolbar" />
          </template>
        </TableHeader>
      </slot>
    </div>
    <div v-loading="loading" class="table-wrapper">
      <el-table
        ref="tableElRef"
        :data="dataSource"
        style="width: 100%"
        :max-height="maxTableHeight"
        v-bind="$attrs"
        border
        stripe
        :size="size"
        v-on="listeners"
        @sort-change="handleSortChange"
      >
        <el-table-column
          v-if="showIndex"
          show-overflow-tooltip
          align="center"
          :index="typeIndex"
          type="index"
          width="50"
        >
          <template slot="header">
            <span>序号</span>
          </template>
        </el-table-column>
        <el-table-column
          v-for="(col) in tableColumns"
          :key="col.key"
          v-bind="{...col}"
          :sortable="sortFun(col.prop) ? 'custom': false"
          show-overflow-tooltip
        >
          <template v-if="col.slotName" #default="scope">
            <slot :name="col.slotName" :scope="scope" />
          </template>
        </el-table-column>
      </el-table>
    </div>

    <div class="pagination-wrapper">
      <el-pagination
        background
        :current-page="pagination.pageNum"
        :page-sizes="[10, 20, 30, 50, 100]"
        :page-size="pagination.pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>
  </div>
</template>

<script>
import TableHeader from './components/TableHeader'
import { throttle } from 'lodash-es'
export default {
  name: 'BaseTable',
  components: {
    TableHeader
  },
  props: {
    showIndex: {
      type: Boolean,
      default: false
    },
    columns: {
      type: Array,
      required: true
    },
    canResize: {
      type: Boolean,
      default: false
    },
    // 底部偏移额外偏移
    bottomResizeOffset: {
      type: Number,
      default: 0
    },
    api: {
      type: Function,
      required: true
    },
    // 排序字段
    sortArr: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      showNoData: false,
      pagination: {
        pageNum: 1,
        pageSize: 10
      },
      dataSource: [],
      total: 0,
      loading: false,
      order: {},
      // 分页器高度
      bottomDefaultOffset: 64,
      maxTableHeight: 0,
      size: 'small',
      tableColumns: []
    }
  },
  computed: {
    listeners: function() {
      return Object.assign({}, this.$listeners)
    }
  },
  created() {
    this.throttleTableHeight = throttle(this.adptiveTableHeight, 100)
    window.addEventListener('resize', this.throttleTableHeight)
    this.tableColumns = this.columns
  },
  mounted() {
    // 固定表格高度
    this.$nextTick(() => {
      this.adptiveTableHeight()
    })
    this.queryData()
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.throttleTableHeight)
  },
  methods: {
    // 获取 slots 内容传递子节点
    getSlot(slots, slot = 'default', data) {
      if (!slots) {
        return null
      }
      const slotFn = slots[slot]
      if (!slotFn) return null
      return slotFn(data)
    },
    handleTableSizeChange(size) {
      this.size = size
    },
    handleColumnChange(values) {
      const _tableColumns = this.columns
      this.tableColumns = _tableColumns.filter(item => {
        if (values.indexOf(item.key) === -1) {
          return false
        }
        return true
      })
    },
    // 自适应表格高度布局
    adptiveTableHeight() {
      const tableRef = this.$refs.tableElRef
      const tableEl = tableRef.$el
      const offsetTop = tableEl?.getBoundingClientRect().top || 0
      const maxHeight = window.innerHeight - offsetTop - this.bottomDefaultOffset - this.bottomResizeOffset
      this.maxTableHeight = maxHeight
    },
    handleSortChange(column) {
      // 单条件排序
      this.order.OrderBy = column.prop
      switch (column.order) {
        case 'ascending':
          this.order.Order = 'ASC'
          break
        case 'descending':
          this.order.Order = 'DESC'
          break
        default:
          this.order.Order = ''
      }
      this.$emit('sortChange', this.order)
      // handle sort change
      this.queryData()
    },
    // 请求数据
    queryData() {
      this.loading = true
      // query 参数
      const params = { ...this.order, ...this.pagination }
      this.api(params).then(res => {
        console.log('res', res)
        this.total = res.data.total
        this.dataSource = res.data.list
        this.$emit('fetch-success')
      }).catch(() => {
        this.dataSource = []
        this.total = 0
      }).finally(() => {
        this.loading = false
      })
    },
    // 重置
    refresh() {
      this.order = {}
      this.pagination = {
        pageNum: 1,
        pageSize: 10
      }
      this.queryData()
    },
    redo() {
      // 刷新当前页数据
      this.order = {}
      this.queryData()
    },
    // 刷新
    sortFun(prop) {
      // 排序
      if (this.sortArr && this.sortArr.length > 0) {
        return this.sortArr.indexOf(prop) > -1
      } else {
        return false
      }
    },
    typeIndex(index) {
      const tabIndex = index + (this.pagination.pageNum - 1) * this.pagination.pageSize + 1
      return tabIndex
    },
    handleSizeChange(size) {
      this.pagination.pageSize = size
      this.$emit('size-change', size)
      this.queryData()
    },
    handleCurrentChange(current) {
      this.pagination.pageNum = current
      this.$emit('current-change', current)
      this.queryData()
    }
  }
}
</script>

<style lang="scss" scoped>
.pagination-wrapper {
  text-align: right;
  padding: 16px 0;
}
.table-header {
  margin-bottom: 16px;
}
</style>
