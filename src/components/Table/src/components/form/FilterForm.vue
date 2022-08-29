<template>
  <el-form :model="formFilters" :inline="true" size="mini">
    <el-row :gutter="24" type="flex" class="row-flex-wrap">
      <el-col
        v-for="field in formFilters"
        :key="field.key"
        :span="field.col || DEFAULT_FILTER_COL"
      >
        <el-form-item :label="field.label">
          <FormField v-model="field.value" v-bind="{...field}" />
        </el-form-item>
      </el-col>
    </el-row>
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
  data() {
    return {
      DEFAULT_FILTER_COL: 8
    }
  },
  computed: {
    formFilters: {
      get() {
        // 计算 formFilters 拆分 布局
        return this.value
      },
      set(value) {
        this.$emit('input', value)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
::v-deep {
  .el-form-item {
    white-space: nowrap;
  }
  .el-form-item__label {
    color: #333;
    font-weight: normal;
  }
}

.row-flex-wrap {
  flex-flow: row wrap;
}
</style>
