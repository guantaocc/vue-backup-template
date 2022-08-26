<template>
  <el-tooltip placement="top">
    <template #content>
      <span>列设置</span>
    </template>
    <el-popover placement="bottom-start" trigger="click">
      <div class="column-setting-content">
        <el-checkbox-group v-model="checkedList" @change="handleCheckChange">
          <el-checkbox v-for="col in cacheList" :key="col.key" :label="col.key">
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
  created() {

  },
  mounted() {
    this.cacheList = this.tableColumns
    this.checkedList = this.cacheList.map(item => item.key)
  },
  methods: {
    handleCheckChange(values) {
      console.log('value', values)
      this.$emit('table-column-change', values)
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
  }
}
</style>
