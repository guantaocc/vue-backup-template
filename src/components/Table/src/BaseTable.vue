<template>
  <div v-loading="loading" class="base-table">
    <div class="table-header">
      <slot name="header">
        <TableHeader />
      </slot>
    </div>
    <div class="table-wrapper">
      <el-table
        ref="tableElRef"
        :data="dataSource"
        style="width: 100%"
        v-bind="$attrs"
        border
        stripe
        v-on="listeners"
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
          v-for="(col) in columns"
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
    // 底部偏移, 已包含分页器和
    bottomResizeOffset: {
      type: Number,
      default: 48
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
      order: {}
    }
  },
  computed: {
    listeners: function() {
      return Object.assign({}, this.$listeners)
    }
  },
  methods: {
    handleSortChange(column) {
      // 单条件排序
      this.order.sortName = column.prop
      switch (column.order) {
        case 'ascending':
          this.order.sortOrder = 'asc'
          break
        case 'descending':
          this.order.sortOrder = 'desc'
          break
        default:
          this.order.sortOrder = ''
      }
      this.$emit('sortChange', this.order)
      // handle sort change
    },
    // 请求数据
    queryData(isReset) {
      this.loading = true
      // query 参数
      const params = { ...this.order, ...this.pagination }
      this.api(params).then(res => {
        this.total = res.total
        this.dataSource = res.data
      }).catch(() => {
        this.dataSource = []
        this.total = 0
      }).finally(() => {
        this.loading = false
      })
    },
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
    },
    handleCurrentChange(current) {
      this.pagination.pageNum = current
      this.$emit('current-change', current)
    }
  }
}
</script>

<style lang="scss" scoped>
.pagination-wrapper {
  text-align: right;
  padding-top: 20px;
}
.table-header {
  margin-bottom: 16px;
}
</style>
