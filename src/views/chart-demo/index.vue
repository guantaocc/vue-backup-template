<template>
  <div>
    <div id="chart" ref="chart" class="chart" />
  </div>
</template>

<script>
import * as echarts from 'echarts'
export default {
  name: 'ChartDemo',
  mounted() {
    this.init()
  },
  methods: {
    init() {
      const myChart = echarts.init(this.$refs.chart)
      const option = {
        title: [
          {
            text: 'Pie label alignTo',
            left: 'center'
          }
        ],
        // tooltip: {
        //   trigger: 'item'
        // },
        legend: {
          orient: 'horizontal',
          bottom: '10%',
          data: [
            {
              name: 'pie1'
            },
            {
              name: 'pie2'
            }
          ]
        },
        series: [
          {
            name: 'pie1',
            type: 'pie',
            radius: '50%',
            center: ['25%', '50%'],
            data: [
              { value: 1048, name: 'Search Engine' },
              { value: 735, name: 'Direct' }
            ]
          },
          {
            name: 'pie2',
            type: 'pie',
            radius: '50%',
            data: [
              { value: 20, name: 'Search Engine1' },
              { value: 80, name: 'Direct1' }
            ],
            center: ['75%', '50%']
          }
        ]
      }
      option && myChart.setOption(option)
      console.log(echarts)
    }
  }
}
</script>

<style lang="scss" scoped>
#chart {
  width: 100%;
  height: 200px;
}
</style>
