<template>
  <div>
    <!-- 品牌管理 -->
    <!-- 按钮 -->
    <el-button type="primary" icon="el-icon-plus" style="margin: 10px 0px"
      >添加</el-button
    >
    <!-- 表格组件 -->
    <!-- data:表格组件将来需要展示的数据-----数组类型 -->
    <!-- border :是给表格添加边框 -->
    <!-- tabel:显示的标题 -->
    <!-- width：对应列的宽度 -->
    <!-- align:标题的对齐方式 -->
    <el-table style="width:100%" border>
       <el-table-column prop="prop" label="序号" width="80px" align="center"></el-table-column>
       <el-table-column prop="prop" label="品牌名称" width="width" align="center"></el-table-column>
       <el-table-column prop="prop" label="品牌Logo" width="width" align="center"></el-table-column>
       <el-table-column prop="prop" label="操作" width="width" ></el-table-column>
    </el-table>

    <!-- 分页器
     page-sizes:代表可以设置每页显示多少条数据
      page-size:代表默认每页显示多少条数据
      layout:代表分页器的布局
      total:代表总共有多少条数据
      @size-change:代表每页显示多少条数据发生改变的时候触发的事件
      @current-change:代表当前页码发生改变的时候触发的事件
    -->
    <el-pagination
    :current-page="6"
    :page-size="3"
    :page-sizes="[3, 6, 9, 12]"
    :total="100"
      background
      layout="prev, pager, next, jumper,->, sizes, total"
      @current-change="handleCurrentChange"
      @size-change="handleSizeChange"
      style="margin-top: 20px;text-align: center;">
    </el-pagination>
  </div>
</template>
<script>
export default {
  name: "TradeMark",
  data() {
    return {};
  },
  created() {},
  computed: {},
  methods: {},
};
</script>
<style lang=
 scoped>
</style>
