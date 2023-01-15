<template>
  <!-- 容器 -->
  <div class="charts" ref="charts"></div>
</template>
<script>
// 引入echarts
import * as echarts from "echarts";
export default {
  data() {
    return {};
  },
  mounted() {
    // 初始化echarts 实例
    let lineCharts = echarts.init(this.$refs.charts);
    lineCharts.setOption({
      xAxis: {
        // 隐藏x轴
        show: false,
        // 最小值和最大值的设置
        min: 0,
        max:100,

      },
      yAxis: {
        // 隐藏y轴
        show: false,
        // 均分
        type: "category",
      },
      // 系列
      series: [
        {
          // 图表的形式为 折线图
          type: "bar",
          data: [78],
          color: 'yellowgreen',
          // 柱状图的宽度
          barWidth: 12,
          // 背景颜色
          showBackground: true,
          backgroundStyle: {
            color:'#eee'
          },
          // 文本
          label: {
            show: true,
            // 改变文本内容
            formatter: '|',
            // 文本标签位置 调整为 右边
            position:'right'
          }

        },
      ],
      // 提示信息
      tooltip:{},
      // 布局
      grid: {
        left: 0,
        right: 0,
        top: 0,
        bottom: 0,
      },
    });
  },
  created() {},
  computed: {},
  methods: {},
};
</script>
<style scoped>
.charts {
  width: 100%;
  height: 100%;
}
</style>
