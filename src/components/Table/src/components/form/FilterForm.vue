<template>
  <el-form :model="formFilters" :inline="true" size="small">
    <el-form-item v-for="field in formFilters" :key="field.key" :label="field.label">
      <FormField v-model="field.value" v-bind="field" />
    </el-form-item>
  </el-form>
</template>

<script>
import FormField from './FormField.vue'
export default {
  name: 'FilterForm',
  components: {
    FormField
  },
  props: {
    value: {
      type: Object,
      required: true
    }
  },
  computed: {
    formFilters: {
      get() {
        return this.value
      },
      set(value) {
        this.$emit('input', value)
      }
    }
  }
}
</script>

<style>

</style>
