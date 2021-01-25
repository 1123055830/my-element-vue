<template>
  <div>
    <eltable
      :tableData="tableData"
      :tableColumns="tableLabel"
      :page="page"
      :rows="rows"
      :total="total"
      :loading="loading"
      :isshow="isshow"
      :tableOption="tableOption"
      @sizeChange="sizeChange"
      @pageChange="pageChange"
      @clickButton="clickButton"
      @sortChange="sortChange"
    >
    <el-form ref="form" id="form" :model="form" label-width="50px">
        <el-form-item label="姓名:">
          <el-input v-model="form.name" clearable></el-input>
        </el-form-item>
        <el-form-item label="年龄:">
          <el-input v-model="form.age" clearable></el-input>
        </el-form-item>
        <el-form-item label="性别:">
          <el-select v-model="form.sex" placeholder="请选择性别" clearable>
            <el-option value="男">男</el-option>
            <el-option value="女">女</el-option>
          </el-select>
        </el-form-item>
      </el-form></eltable>
  </div>
</template>

<script>
import Eltable from "@/components/Eltable";
export default {
  components: { Eltable },
  data() {
    return {
     loading: false,
     isshow: true,
     page: 1,
     rows: 20,
     total: 100,
     form: { name: "", age: "", sex: "" },
     // 表头数据
     tableLabel: [
       {
         label: "姓名",
         param: "name"
       },
       {
         label: "性别",
         param: "sex",
         render: row => {
           if (row.sex === 0) {
             return "女";
           } else if (row.sex === 1) {
             return "男";
           }
         }
       },
       {
         label: "地点",
         param: "address"
       },
       {
         label: "时间",
         sortable: true,
         param: "date"
       }
     ],
     // 表格操作
     tableOption: {
       label: "操作",
       options: [
         {
           label: "编辑",
           type: "primary",
           icon: "el-icon-delete",
           methods: "del"
         },
         {
           label: "删除",
           type: "danger",
           icon: "el-icon-delete",
           methods: "del"
         }
       ]
     },
     // 表格数据
     tableData: [
       {
         date: "2016-05-02",
         name: "王小虎",
         address: "上海市普陀区金沙江路 1518 弄",
         sex: 0
       },
       {
         date: "2016-05-04",
         name: "王小虎",
         address: "上海市普陀区金沙江路 1517 弄",
         sex: 1
       },
       {
         date: "2016-05-01",
         name: "王小虎",
         address: "上海市普陀区金沙江路 1519 弄",
         sex: 0
       },
       {
         date: "2016-05-03",
         name: "王小虎",
         address: "上海市普陀区金沙江路 1516 弄",
         sex: 1
       }
     ]
    };
  },
  methods: {
    // 切换当前一页展示多少条
    sizeChange(val) {
      this.rows = val;
      console.log(`每页 ${val} 条`);
    },
    // 翻页
    pageChange(val) {
      this.page = val;
      console.log(`当前页: ${val}`);
    },
    // 点击事件
    clickButton(val) {
      // 调用事件
      this[val.methods](val.row);
    },
    // 排序
    sortChange(val) {
      console.log(val);
    },
    del(val) {
      // 我是删除
      console.log(val);
    }
  }
};
</script>

<style scoped>
</style>