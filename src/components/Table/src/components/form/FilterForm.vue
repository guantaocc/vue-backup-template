<template>
  <el-form ref="formRef" :model="formFilters" :inline="true" size="small">
    <el-row :gutter="24" type="flex" class="row-flex-wrap">
      <el-col
        v-for="field in filters"
        :key="field.key"
        :span="field.col || DEFAULT_FILTER_COL"
      >
        <el-form-item :label="field.label" :prop="field.key" :rules="field.rules">
          <FormField v-model="formFilters[field.key]" v-bind="{...field}" />
        </el-form-item>
      </el-col>
      <el-col :span="8">
        <el-form-item>
          <el-button type="plain" @click="resetFilter">重置</el-button>
          <el-button type="primary" @click="searchFilter">查询</el-button>
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
  },
  methods: {
    searchFilter() {
      console.log(this.formFilters)
      // 校验查询条件
      this.$refs.formRef.validate((valid) => {
        if (valid) {
          this.$emit('search-filters')
        }
      })
    },
    resetFilter() {
      this.$refs.formRef.resetFields()
      this.$emit('reset-filters')
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
