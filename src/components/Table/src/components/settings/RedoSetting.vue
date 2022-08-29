<template>
  <el-tooltip placement="top">
    <template #content>
      <span>刷新</span>
    </template>
    <span>
      <svg-icon icon-class="redo" @click="redoTable" />
    </span>
  </el-tooltip>
</template>

<script>
export default {
  name: 'RedoSetting',
  methods: {
    redoTable() {
      this.$emit('redo')
    }
  }
}
</script>

<style lang="scss">

</style>
