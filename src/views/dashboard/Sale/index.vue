<template>
  <div>
    <el-card class="box-card" style="margin: 10px 0px">
      <div slot="header" class="clearfix">
        <!-- v-model="activeName" @tab-click="handleClick" -->
        <el-tabs >
          <el-tab-pane label="销售额" name="first"></el-tab-pane>
          <el-tab-pane label="访问量" name="second"></el-tab-pane>

        </el-tabs>
      </div>
    </el-card>
  </div>
</template>
<script>
export default {
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
