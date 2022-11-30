<template>
  <div>
    <!-- 添加SPU|修改SPU -->
    <el-form ref="form" label-width="80px" :model="spu">
      <el-form-item label="SPU名称">
        <el-input placeholder="SPU名称" v-model="spu.spuName"></el-input>
      </el-form-item>

      <el-form-item label="品牌">
        <el-select placeholder="请选择品牌" v-model="spu.tmId">
          <el-option :label="tm.tmName" :value="tm.id" v-for="(tm,index) in tradeMarkList" :key="tm.id"></el-option>
        </el-select>
      </el-form-item>

      <el-form-item label="SPU描述">
        <el-input placeholder="SPU描述" type="textarea" rows="4" v-model="spu.description"></el-input>
      </el-form-item>

      <el-form-item label="SPU图片">
         <!-- 上传图片：action图片上传的地址  list-type: 文件列表的类型 on-preview:图片预览的时候会触发  on-remove:当删除图片的时候会触发
         file-list：照片墙需要展示的数据【数组：数组里面的元素务必要有name、url属性】
         on-preview：图片预览功能
         on-remove:删除图片的时候会触发
        -->
        <el-upload
          action="/dev-api/admin/product/fileUpload"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview"
          :on-remove="handleRemove"
          :file-list="spuImageList"
        >
          <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt="" />
        </el-dialog>
      </el-form-item>

      <el-form-item label="销售属性">
        <el-select placeholder="还有3未选择" value="">
          <el-option></el-option>
        </el-select>
        <el-button style="margin:0px 10px" type="primary" icon="el-icon-plus">添加销售属性</el-button>
          <el-table style="width:100%" border>
          <el-table-column label="序号" type="index" align="center" width="80"></el-table-column>
          <el-table-column label="属性名" prop="prop" width="width"></el-table-column>
          <el-table-column label="属性值名称列表" prop="prop" width="width"></el-table-column>
          <el-table-column label="操作" prop="prop" width="width"></el-table-column>
      </el-table>
      </el-form-item>

      <el-form-item >
        <el-button type="primary">保存</el-button>
        <el-button @click="$emit('changeScene',0)">取消</el-button>
      </el-form-item>

    </el-form>
  </div>
</template>
<script>
import ItemVue from '@/layout/components/Sidebar/Item.vue';
export default {
  data() {
    return {
      dialogImageUrl: "",
      dialogVisible: false,
      // spu: {},//存储spu信息属性
      //spu属性初始化的时候是一个空的对象,在修改SPU的时候，会想服务器发请求，返回SPU信息（对象），在修改的时候可以利用服务器返回的这个对象收集最新的数据提交给服务器
      //添加SPU，如果是添加SPU的时候并没有向服务器发请求，数据收集到哪里呀[SPU]，收集数据的时候有哪些字段呀，看文档
      spu: {
        //三级分类的id
        category3Id: 0,
        //描述
        description: "",
        //spu名称
        spuName: "",
        //平台的id
        tmId: "",
        //收集SPU图片的信息
        spuImageList: [
          // {
          //   id: 0,
          //   imgName: "string",
          //   imgUrl: "string",
          //   spuId: 0,
          // },
        ],
        //平台属性与属性值收集
        spuSaleAttrList: [
          // {
          //   baseSaleAttrId: 0,
          //   id: 0,
          //   saleAttrName: "string",
          //   spuId: 0,
          //   spuSaleAttrValueList: [
          //     {
          //       baseSaleAttrId: 0,
          //       id: 0,
          //       isChecked: "string",
          //       saleAttrName: "string",
          //       saleAttrValueName: "string",
          //       spuId: 0,
          //     },
          //   ],
          // },
        ],
      },
      tradeMarkList: [],//存储品牌信息
      spuImageList: [],//存储SPU图片的数据
      saleAttrList:[],//销售属性的数据（项目全部的销售属性）
    };
  },
  created() {},
  computed: {},
  methods: {
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    //初始化SpuForm 数据
    async initSpuData(spu) {
    // 获取spu 信息的数据
     let spuResult = await this.$API.spu.reqSpu(spu.id);
     if (spuResult.code == 200) {
       this.spu = spuResult.data;
     }
    //  获取品牌信息
      let tradeMarkResult = await this.$API.spu.reqTradeMarkList();
      if (tradeMarkResult.code == 200) {
        this.tradeMarkList = tradeMarkResult.data;
      }
      // 获取spu 图片的数据
      let spuImageResult = await this.$API.spu.reqSpuImageList(spu.id);
      if (spuImageResult.code==200) {
        // this.spuImageList = spuImageResult.data;
        let listArr = spuImageResult.data;
        //由于照片墙显示图片的数据需要数组，数组里面的元素需要有name与url字段
        //需要把服务器返回的数据进行修改
        listArr.forEach(item => {
          item.name = item.imgName;
          item.url = item.imgUrl;
        });
        this.spuImageList = listArr;
      }
      // 获取平台全部的销售属性
      let saleResult = await this.$API.spu.reqBaseSaleAttrList();
      if (saleResult.code == 200) {
        this.saleAttrList = saleResult.data;
      }

    }
  },
};
</script>
<style lang=
scoped>
</style>
