<template>
  <el-form :model="formFilters" :inline="true" size="mini">
    <el-row :gutter="24" type="flex" class="row-flex-wrap">
      <el-col
        v-for="field in filters"
        :key="field.key"
        :span="field.col || DEFAULT_FILTER_COL"
      >
        <el-form-item :label="field.label">
          <FormField v-model="formFilters[field.key]" v-bind="{...field}" />
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
    },
    // 配置的 filters
    filters: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      DEFAULT_FILTER_COL: 8,
      initFormFilters: {}
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
