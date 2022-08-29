<template>
  <el-tooltip placement="top">
    <template #content>
      <span>列设置</span>
    </template>
    <el-popover placement="bottom-start" trigger="click" popper-class="basetable-column-setting-popper">
      <div class="column-setting-content">
        <el-checkbox-group ref="columnCheckBox" v-model="checkedList" @change="handleCheckChange">
          <el-checkbox v-for="col in cacheList" :key="col._key" class="sortable-checkbox-item" :label="col._key">
            {{ col.label }}
          </el-checkbox>
        </el-checkbox-group>
      </div>
      <span slot="reference">
        <svg-icon icon-class="setting" />
      </span>
    </el-popover>
  </el-tooltip>
</template>

<script>
import Sortable from 'sortablejs'
export default {
  name: 'ColumnSetting',
  props: {
    tableColumns: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      checkedList: [],
      cacheList: []
    }
  },
  mounted() {
    this.cacheList = this.tableColumns
    this.checkedList = this.cacheList.map(item => item._key)
    // init sortable
    this.initSortable()
  },
  methods: {
    // 获取checkList顺序
    getSortedCheckList() {
      const result = []
      this.cacheList.forEach(item => {
        const index = this.checkedList.findIndex(key => item._key === key)
        if (index !== -1) {
          result.push(item)
        }
      })
      return result.map(item => {
        return {
          ...item,
          key: `${item.key}-${new Date().getTime()}`
        }
      })
    },
    initSortable() {
      const refEl = this.$refs.columnCheckBox.$el
      const _this = this
      let draggedDom = null
      new Sortable(refEl, {
        animation: 150,
        ghostClass: 'blue-background-class',
        preventOnFilter: true,
        dragClass: 'column-sortable-drag',
        onMove: function(evt) {
          draggedDom && draggedDom.classList.remove('column-sortable-dragged')
          draggedDom = evt.dragged
          draggedDom && draggedDom.classList.add('column-sortable-dragged')
        },
        onEnd: function(evt) {
          draggedDom && draggedDom.classList.remove('column-sortable-dragged')
          const oldIndex = evt.oldIndex
          const newIndex = evt.newIndex
          // 删除旧元素，移动新元素
          const oldItem = _this.cacheList[oldIndex]
          const newItem = _this.cacheList[newIndex]
          _this.cacheList.splice(newIndex, 1, oldItem)
          _this.cacheList.splice(oldIndex, 1, newItem)
          _this.$emit('table-column-change', _this.getSortedCheckList())
        }
      })
    },
    handleCheckChange() {
      // 触发排序 change
      this.$emit('table-column-change', this.getSortedCheckList())
    }
  }
}
</script>

<style lang="scss" scoped>
.el-tooltip {
  height: 24px;
  margin-left: 16px;
  cursor: pointer;
  outline: none;
}
.column-setting-content {
  .el-checkbox-group {
    display: flex;
    flex-direction: column;
    padding: 4px;
    .sortable-checkbox-item {
      ::v-deep {
        .el-checkbox__label {
          color: #323232;
        }
      }
      box-sizing: border-box;
      width: 100%;
      padding: 4px 0 4px 24px;
      position: relative;
      &::before {
        content: "";
        position: absolute;
        left: 2px;
        top: 6px;
        cursor: grab;
        font-family: element-icons!important;
        font-style: normal;
        transition: opacity .2s ease-in-out;
        color: #989898;
        font-weight: 400;
        font-variant: normal;
        text-transform: none;
        line-height: 1;
        vertical-align: baseline;
        display: inline-block;
        -webkit-font-smoothing: antialiased;
      }
    }
    .sortable-checkbox-item:hover {
      background-color: #e6f7ff;
    }
  }
  .column-sortable-dragged {
    border: 1px solid #1890ff;
  }
}

</style>
<style>
.basetable-column-setting-popper {
  padding: 0;
}
</style>
