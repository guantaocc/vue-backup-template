<template>
  <div class="table-header-wrapper">
    <div class="table-title">
      <slot name="tableTitle">
        <div class="title">基础表格</div>
      </slot>
    </div>
    <div class="table-action-panel">
      <slot name="toolbar" />
      <TableSetting v-bind="$attrs" v-on="$listeners" />
    </div>
  </div>
</template>

<script>
import TableSetting from './TableSetting.vue'
export default {
  name: 'TableHeader',
  components: {
    TableSetting
  }
}
</script>

<style lang="scss" scoped>
.table-header-wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.table-action-panel {
  display: flex;
  font-size: 24px;
}
</style>
