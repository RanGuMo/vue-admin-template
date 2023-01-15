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
          type: "line",
          // 平滑的折线
          smooth:true,
          data: [12, 45,19,64,23, 7, 32, 7, 78],
          // 拐点样式的设置
          itemStyle: {
            opacity: 0,
          },
          // 线条样式
          lineStyle: {
            color: "purple",
          },
          // 区域填充颜色
          areaStyle: {
            color: {
              type: "linear",
              x: 0,
              y: 0,
              x2: 0,
              y2: 1,
              colorStops: [
                {
                  offset: 0,
                  color: "purple", // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: "#fff", // 100% 处的颜色
                },
              ],
              global: false, // 缺省为 false
            },
          },
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
