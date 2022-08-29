<template>
  <div class="table-setting">
    <RedoSetting v-on="$listeners" />
    <SizeSetting v-on="$listeners" />
    <ColumnSetting v-bind="$attrs" v-on="$listeners" />
  </div>
</template>

<script>
import RedoSetting from './settings/RedoSetting.vue'
import SizeSetting from './settings/SizeSetting.vue'
import ColumnSetting from './settings/ColumnSetting.vue'
export default {
  components: {
    RedoSetting,
    SizeSetting,
    ColumnSetting
  }
}
</script>

<style lang="scss" scoped>
.table-setting {
  display: flex;
  align-items: center;
  height: 16px;
  font-size: 16px;
  ::v-deep {
    .svg-icon {
      width: 16px;
      height: 16px;
      color: #333;
      margin-left: 12px;
      cursor: pointer;
      outline: none;
      vertical-align: 0;
    }
  }
}
</style>
