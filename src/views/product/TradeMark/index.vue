<template>
  <div>
    <!-- 品牌管理 -->
    <!-- 按钮 -->
    <el-button
      type="primary"
      icon="el-icon-plus"
      style="margin: 10px 0px"
      @click="showDialog"
      >添加</el-button
    >
    <!-- 表格组件 -->
    <!-- data:表格组件将来需要展示的数据-----数组类型 -->
    <!-- border :是给表格添加边框 -->
    <!-- tabel:显示的标题 -->
    <!-- width：对应列的宽度 -->
    <!-- align:标题的对齐方式 -->
    <!-- elementui 展示数据是一列一列展示的 -->
    <!-- prop：对应列内容的字段名 -->
    <el-table style="width: 100%" border :data="list">
      <el-table-column
        type="index"
        label="序号"
        width="80px"
        align="center"
      ></el-table-column>
      <el-table-column prop="tmName" label="品牌名称" width="width">
      </el-table-column>
      <el-table-column prop="logoUrl" label="品牌Logo" width="width">
        <template slot-scope="{ row, $index }">
          <img :src="row.logoUrl" style="width: 100px; height: 80px" />
        </template>
      </el-table-column>
      <el-table-column prop="prop" label="操作" width="width">
        <template slot-scope="{ row, $index }">
          <el-button
            type="warning"
            icon="el-icon-edit"
            size="mini"
            @click="updateTradeMark(row)"
            >修改</el-button
          >
          <el-button
            type="danger"
            icon="el-icon-delete"
            size="mini"
            @click="deleteTradeMark(row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
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
      :current-page="page"
      :page-size="limit"
      :page-sizes="[3, 6, 9, 12]"
      :total="total"
      background
      @current-change="getPageList"
      @size-change="handleSizeChange"
      layout="prev, pager, next, jumper,->, sizes, total"
      style="margin-top: 20px; text-align: center"
    >
    </el-pagination>

    <!-- 对话框
    :visible.sync ：控制对话框的显示与隐藏
    Form 组件提供了表单验证的功能，只需要通过 rules 属性传入约定的验证规则，并将 Form-Item 的 prop 属性设置为需校验的字段名即可。
    -->
    <el-dialog
      :title="tmForm.id ? '修改品牌' : '添加品牌'"
      :visible.sync="dialogFormVisible"
    >
      <!-- form 表单  :model 属性，这个属性的作用是，把表单的数据收集到哪个对象身上，表单验证也需要用到这个属性-->
      <el-form style="width: 80%" :model="tmForm" :rules="rules" ref="ruleForm">
        <el-form-item label="品牌名称" label-width="100px" prop="tmName">
          <el-input autocomplete="off" v-model="tmForm.tmName"></el-input>
        </el-form-item>
        <el-form-item label="品牌Logo" label-width="100px" prop="logoUrl">
          <!--这里收集数据：不能使用v-model，因为不是表单元素
            action:设置图片上传的地址
            :on-success:可以检测到图片上传成功，当图片上传成功，会执行一次
            :before-upload：可以在上传图片之前，会执行一次

          -->
          <el-upload
            class="avatar-uploader"
            action="/dev-api/admin/product/fileUpload"
            :show-file-list="false"
            :on-success="handleAvatarSuccess"
            :before-upload="beforeAvatarUpload"
          >
            <img v-if="tmForm.logoUrl" :src="tmForm.logoUrl" class="avatar" />
            <i v-else class="el-icon-plus avatar-uploader-icon"></i>
            <div slot="tip" class="el-upload__tip">
              只能上传jpg/png文件，且不超过500kb
            </div>
          </el-upload>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="addOrUpdateTradeMark"
          >确 定</el-button
        >
      </div>
    </el-dialog>
  </div>
</template>
<script>
export default {
  name: "TradeMark",
  data() {
    //自定义校验规则
    var validateTmName = (rule, value, callback) => {
      //自定义校验规则
      if (value.length < 2 || value.length > 10) {
        callback(new Error("品牌名称的长度在2-10位之间"));
      } else {
        callback();
      }
    };

    return {
      // 当前分页器在第几页
      page: 1,
      // 当前页数展示的数据条数
      limit: 3,
      // 总条数
      total: 0,
      // 品牌列表数据
      list: [],
      // 对话框显示与隐藏
      dialogFormVisible: false,
      // 收集品牌的信息：对象身上的属性，需要和后台提供的字段一致
      tmForm: {
        tmName: "",
        logoUrl: "",
      },
      // 表单验证规则
      rules: {
        // 品牌名称的验证规则
        tmName: [
          { required: true, message: "请输入品牌名称", trigger: "blur" },
          // {
          //   min: 2,
          //   max: 10,
          //   message: "长度在 2 到 10 个字符",
          //   trigger: "change",
          // },
          //自定义校验规则
          { validator: validateTmName, trigger: "change" },
        ],
        // 品牌logo的验证规则
        logoUrl: [{ required: true, message: "请选择品牌Logo" }],
      },
    };
  },
  created() {},
  computed: {},
  // 当组件挂载完毕，发起请求
  mounted() {
    // 获取品牌列表数据的方法
    this.getPageList();
  },
  methods: {
    // 获取品牌列表的数据
    async getPageList(pager = 1) {
      // 当不传值时，pager默认为1,
      this.page = pager;
      // 解构出参数
      const { page, limit } = this;
      // 获取品牌列表的接口
      let result = await this.$API.trademark.reqTradeMarkList(page, limit);
      //console.log(result);
      if (result.code == 200) {
        this.total = result.data.total;
        this.list = result.data.records;
      }
    },
    handleSizeChange(limit) {
      this.limit = limit;
      this.getPageList();
    },

    // handleCurrentChange(pager) {
    //   this.page = pager;
    //   this.getPageList();
    // }
    // 点击添加品牌的按钮
    showDialog() {
      // 显示对话框
      this.dialogFormVisible = true;
      // 清空表单数据
      this.tmForm = { tmName: "", logoUrl: "" };
    },
    // 修改按钮的单击事件
    updateTradeMark(row) {
      // row :当前用户选中的这个品牌的信息
      // console.log(row);
      // 显示对话框
      this.dialogFormVisible = true;
      // 将已有的品牌信息赋值给tmForm 进行展示
      //将服务器返回品牌的信息，直接赋值给了tmForm进行展示。
      //也就是tmForm存储即为服务器返回品牌信息
      // this.tmForm = row;   这个写法不行，当我修改信息时。服务器也修改了，哪怕你没有点确定按钮
      this.tmForm = { ...row }; //必须要用到浅拷贝
    },
    // 上传图片相关的回调
    // 图片上传成功
    handleAvatarSuccess(res, file) {
      console.log(res);
      this.tmForm.logoUrl = res.data;
    },
    // 图片上传之前
    beforeAvatarUpload(file) {
      const isJPG = file.type === "image/jpeg";
      const isLt2M = file.size / 1024 / 1024 < 2;

      if (!isJPG) {
        this.$message.error("上传头像图片只能是 JPG 格式!");
      }
      if (!isLt2M) {
        this.$message.error("上传头像图片大小不能超过 2MB!");
      }
      return isJPG && isLt2M;
    },
    // 添加或修改品牌
    async addOrUpdateTradeMark() {
      // 进行表单校验，当全部字段验证通过，再去书写业务逻辑
      this.$refs.ruleForm.validate(async (success) => {
        // 如果全部字段符合条件
        if (success) {
          // 关闭对话框
          this.dialogFormVisible = false;
          // 发起请求（添加品牌|修改品牌）
          let result = await this.$API.trademark.reqAddOrUpdateTradeMark(
            this.tmForm
          );
          if (result.code == 200) {
            // 弹出信息：添加品牌成功 | 修改品牌成功
            this.$message({
              type: "success",
              message: this.tmForm.id ? "修改品牌成功" : "添加品牌成功",
            });
            // 添加或者修改品牌成功之后，需要再次获取品牌列表进行展示
            // 如果添加品牌：停留在第一页，修改品牌应该留在当前页
            this.getPageList(this.tmForm.id ? this.page : 1);
          }
        } else {
          console.log("error submit!");
          return false;
        }
      });
    },
    // 删除品牌的操作
    deleteTradeMark(row) {
      // console.log(row);
      this.$confirm(`你确定要删除${row.tmName}?`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(async () => {
          // 当用户点击确定按钮会触发
          // 向服务器发请求
          let result = await this.$API.trademark.reqDeleteTradeMark(row.id);
          // 如果删除成功
          if (result.code == 200) {
            this.$message({
              type: "success",
              message: "删除成功!",
            });
            // 再次获取品牌列表进行展示
            // 如果当前页的数据大于1，删除后，仍然停留在当前页，否则回到上一页
            this.getPageList(this.list.length>1?this.page:this.page-1);
          }
        })
        .catch(() => {
          // 当用户点击取消按钮会触发
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    },
  },
};
</script>
<style>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>
