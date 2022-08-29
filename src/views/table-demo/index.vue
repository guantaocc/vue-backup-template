<template>
  <div class="table-container">
    <BaseTable :api="fetchList" :columns="columns" :sort-arr="['timestamp']" :filters="filters" show-index is-search-form>
      <template #operation>
        <el-button type="text" size="small">新增</el-button>
      </template>
      <template #tableTitle>
        <div>基础表格用法</div>
      </template>
    </BaseTable>
  </div>
</template>

<script>
import { BaseTable } from '@/components/Table'
import { fetchList } from '@/api/article'
export default {
  name: 'TableDemo',
  components: {
    BaseTable
  },
  data() {
    return {
      fetchList,
      filters: [
        {
          label: '文章标题',
          value: '',
          key: 'title',
          // 渲染组件
          component: 'input',
          props: {
            //
            rules: [
              {
                required: true,
                trigger: 'blur'
              }
            ]
          }
        },
        {
          label: '状态',
          value: '',
          key: 'status',
          // 渲染组件
          component: 'select',
          props: {
            options: [
              {
                value: '选项1',
                label: '黄金糕'
              }, {
                value: '选项2',
                label: '双皮奶'
              }
            ]
          }
        },
        {
          label: '创建时间',
          key: 'createtime',
          value: [],
          component: 'date-picker',
          props: {
            type: 'daterange',
            'range-separator': '至',
            'start-placeholder': '开始日期',
            'end-placeholder': '结束日期',
            'value-format': 'yyyy-MM-dd'
          },
          // 将值转换搜索的条件
          search: {
            transform: value => {
              return {
                startTime: value[0],
                endTime: value[1]
              }
            }
          }
        }
      ],
      columns: [
        {
          label: '文章标题',
          prop: 'title',
          key: 'title',
          width: 180
        },
        {
          label: '作者',
          prop: 'author',
          key: 'author',
          width: 180
        },
        {
          label: '接收人',
          prop: 'reviewer',
          key: 'reviewer'
        },
        {
          label: '内容',
          prop: 'content',
          key: 'content'
        },
        {
          label: '时间',
          prop: 'timestamp',
          key: 'timestamp'
        },
        {
          label: '操作',
          key: 'operation',
          fixed: 'right',
          slotName: 'operation',
          width: 100
        }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
.table-container {
  padding: 24px 24px 0 24px;
}
</style>
