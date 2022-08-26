<template>
  <div class="table-container">
    <BaseTable :api="fetchList" :columns="columns" :sort-arr="['timestamp']" show-index>
      <template #operation>
        <el-button type="text" size="small">新增</el-button>
      </template>
      <template #tableTitle>
        <div>this is slot table title</div>
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
