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
        // 均分
        type: "category",
      },
      yAxis: {
        // 隐藏y轴
        show: false,
      },
      // 系列
      series: [
        {
          // 图表的形式为 折线图
          type: "bar",
          data: [12, 45, 19, 64, 23, 7, 32, 7, 78,92,9,21],
          color:'cyan',

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
