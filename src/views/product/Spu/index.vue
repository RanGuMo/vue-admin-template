<template>
  <div>
    <!-- Spu -->
    <el-card style="margin: 20px 0px">
      <CategorySelect @getCategoryId="getCategoryId" :show="(scene!=0)" />
    </el-card>
    <el-card>
      <!-- 底部这里将来是有三部分进行切换 -->
      <div v-show="scene==0">
        <el-button type="primary" icon="el-icon-plus" style="margin: 10px 0px"
          :disabled="!category3Id" @click="addSpu">添加SPU</el-button
        >
        <el-table style="width: 100%" border :data="records">
          <el-table-column
            label="序号"
            width="80"
            type="index"
            align="center"
          ></el-table-column>
          <el-table-column
            label="SPU名称"
            prop="spuName"
            width="width"
          ></el-table-column>
          <el-table-column
            label="SPU描述"
            prop="description"
            width="width"
          ></el-table-column>
          <el-table-column label="操作" prop="prop" width="width">
            <template slot-scope="{ row, $index }">
              <hint-button
                type="success"
                size="mini"
                icon="el-icon-plus"
                title="添加Sku"
              ></hint-button>
              <hint-button
                type="warning"
                size="mini"
                icon="el-icon-edit"
                title="修改Spu" @click="updateSpu(row)"
              ></hint-button>
              <hint-button
                type="info"
                size="mini"
                icon="el-icon-info"
                title="查看当前Spu全部的Sku列表"
              ></hint-button>
              <hint-button
                type="danger"
                size="mini"
                icon="el-icon-delete"
                title="删除SPU"
              ></hint-button>
            </template>
          </el-table-column>
        </el-table>
        <!-- 分页器     @current-change="getPageList"
        -->
        <el-pagination
          :current-page="page"
          :page-size="limit"
          :page-sizes="[3, 6, 9, 12]"
          :total="total"
          background
          @current-change="getSpuList"
          @size-change="handleSizeChange"
          layout="prev, pager, next, jumper,->, sizes, total"
          style="margin-top: 20px; text-align: center"
        >
        </el-pagination>
      </div>
      <SpuForm v-show="scene==1" @changeScene="changeScene" ref="spu"></SpuForm>
      <SkuForm v-show="scene==2"></SkuForm>
    </el-card>
  </div>
</template>
<script>
// 引入子组件
import SkuForm from './SkuForm';
import SpuForm from './SpuForm';
export default {
  name: "Spu",
  components: {
    SkuForm,
    SpuForm
  },
  data() {
    return {
      //分别是分类的id
      category1Id: "",
      category2Id: "",
      category3Id: "",
      //控制三级联动的可操作性
      // show: true,
      page: 1, //分页器当前第几页
      limit: 3, //每一页需要展示多少条数据
      records: [], //spu列表的数据
      total: 0, //分页器一共需要展示数据的条数
      scene: 0, //0代表展示SPU列表数据   1 添加SPU|修改SPU   2 添加SKU
    };
  },
  created() {},
  computed: {},
  methods: {
    // 点击 分页器上的页号，进行数据展示（可以不写这个函数，而是结合getSpuList一起使用）
    // getPageList(page) {
    //   this.page = page;
    //   this.getSpuList();
    // },
    //三级联动的自定义事件，可以把子组件的相应的id传递给父组件
    getCategoryId({ categoryId, level }) {
      //categoryId:获取到一、二、三级分类的id  level：为了区分是几级id
      if (level == 1) {
        this.category1Id = categoryId;
        //清除2、3级分类的id
        this.category2Id = "";
        this.category3Id = "";
      } else if (level == 2) {
        this.category2Id = categoryId;
        //清除3级id
        this.category3Id = "";
      } else {
        this.category3Id = categoryId;
        //获取SPU列表数据进行展示
        this.getSpuList();
      }
    },
    //获取SPU列表数据的方法
    async getSpuList(pages = 1) {
      this.page = pages;
      const { page, limit, category3Id } = this;
      //携带三个参数:page 第几页  limit 每一页需要展示多少条数据  三级分类id
      let result = await this.$API.spu.reqSpuList(page, limit, category3Id);
      if (result.code == 200) {
        this.total = result.data.total;
        this.records = result.data.records;
      }
    },
    //当分页器的某一个展示数据条数发生变化的回调
    handleSizeChange(limit) {
      this.limit = limit;
      this.getSpuList();
    },
    // 添加Spu 按钮的回调
    addSpu() {
      this.scene = 1;
    },
    // 修改某一个spu
    updateSpu(row) {
      this.scene = 1;
      //获取子组件SpuForm子组件的
      //在父组件当中可以通过$ref获取子组件等等
      this.$refs.spu.initSpuData(row);
    },
    // 取消按钮的自定义事件的回调 （SpuForm）
    changeScene(scene) {
      // 切换场景（结构）
      this.scene = scene;
    }
  },
};
</script>
<style lang=
 scoped>
</style>
