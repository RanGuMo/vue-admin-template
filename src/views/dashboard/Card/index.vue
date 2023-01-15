<template>
  <div>
    <el-row>
      <el-col :span="6"
        ><el-card style="margin: 10px 10px"
          ><Detail title="总销售额" count="￥ 126560">
              <template slot="charts">
                <span>周同比 56.67% <svg t="1673673887915" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="6260" width="16" height="16"><path d="M960 704L512 256l-448 448z" fill="#d81e06" p-id="6261"></path></svg> </span>
                <span>日同比 19.96% <svg t="1673673830114" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="5121" width="16" height="16"><path d="M64 320l448 448 448-448z" fill="#1afa29" p-id="5122"></path></svg></span>
              </template>
              <template slot="footer">
                <span>日销售额￥ 12423</span>
              </template>

          </Detail></el-card
      ></el-col>
      <el-col :span="6"
        ><el-card style="margin: 10px 10px"
          ><Detail title="访问量" count="88460">

            <template slot="charts">
              <lineChart/>
            </template>
            <template slot="footer">
                <span>日访问量 1234</span>
              </template>
          </Detail></el-card
      ></el-col>
      <el-col :span="6"
        ><el-card style="margin: 10px 10px"
          ><Detail title="支付笔数" count="88460">

            <template slot="charts">
              <barChart/>
            </template>
            <template slot="footer">
                <span>转化率 65%</span>
              </template>
          </Detail></el-card
      ></el-col>
      <el-col :span="6"
        ><el-card style="margin: 10px 10px"
          ><Detail title="运营活动效果" count="78%">
            <template slot="charts">
              <progressChart/>
            </template>
            <template slot="footer">
              <span>周同比 12% <svg t="1673673887915" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="6260" width="16" height="16"><path d="M960 704L512 256l-448 448z" fill="#d81e06" p-id="6261"></path></svg> </span>
                <span>日同比 11% <svg t="1673673830114" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="5121" width="16" height="16"><path d="M64 320l448 448 448-448z" fill="#1afa29" p-id="5122"></path></svg></span>
              </template>
          </Detail></el-card
      ></el-col>
    </el-row>
  </div>
</template>
<script>
import Detail from "./Detail";
import lineChart from './lineChart'
import barChart from './barChart'
import progressChart from './progressChart'
export default {
  data() {
    return {};
  },
  components: {
    Detail,
    lineChart,
    barChart,
    progressChart,
  },
  created() {},
  computed: {},
  methods: {},
};
</script>
<style scoped>
</style>
