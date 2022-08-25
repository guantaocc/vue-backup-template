<template>
  <div class="table-container">
    <BaseTable :data-source="dataSource" :columns="columns" :sort-arr="['age']" show-index>
      <template #operation>
        <el-button type="text" size="small">新增</el-button>
      </template>
    </BaseTable>
  </div>
</template>

<script>
import { BaseTable } from '@/components/Table'
export default {
  name: 'TableDemo',
  components: {
    BaseTable
  },
  data() {
    return {
      dataSource: [
        {
          key: '1',
          name: '胡彦斌',
          age: 32,
          address: '西湖区湖底公园1号'
        },
        {
          key: '2',
          name: '胡彦祖',
          age: 42,
          address: '西湖区湖底公园1号'
        },
        {
          key: '3',
          name: '胡彦祖',
          age: 42,
          address: '西湖区湖底公园1号'
        }
      ],
      columns: [
        {
          label: '姓名',
          prop: 'name',
          key: 'name',
          width: 180
        },
        {
          label: '年龄',
          prop: 'age',
          key: 'age',
          width: 180
        },
        {
          label: '住址',
          prop: 'address',
          key: 'address'
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
