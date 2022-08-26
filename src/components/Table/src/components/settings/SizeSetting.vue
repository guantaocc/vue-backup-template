<template>
  <el-tooltip placement="top">
    <template #content>
      <span>密度</span>
    </template>
    <el-dropdown placement="bottom" trigger="click" @command="handleCommand">
      <span>
        <svg-icon icon-class="column-height" />
      </span>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item command="mini">紧凑</el-dropdown-item>
        <el-dropdown-item command="small">中等</el-dropdown-item>
        <el-dropdown-item command="medium">宽松</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </el-tooltip>
</template>

<script>
export default {
  name: 'SizeSetting',
  methods: {
    handleCommand(command) {
      this.$emit('table-size-change', command)
    }
  }
}
</script>

<style lang="scss" scoped>
  .el-tooltip{
    height: 24px;
    font-size: 24px;
    color: #333;
    margin-left: 12px;
    cursor: pointer;
    outline: none;
    ::v-deep {
      .el-dropdown-selfdefine {
        display: inline-block;
        height: 24px;
        line-height: 24px;
      }
    }
  }
</style>
