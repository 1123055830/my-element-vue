<template>
  <div class="table">
    <div class="searchBox">
      <!-- 查询表单 -->
      <slot></slot>
      <div class="btn">
        <el-button type="primary" @click="search">查询</el-button>
        <el-button type="primary" @click="refresh">重置</el-button>
      </div>
    </div>
    <!-- 表格开始 -->
    <el-table
      ref="table"
      :header-cell-style="{
        fontSize: '14px',
        background: '#fafafa',
        color: 'black',
      }"
      v-loading="loading"
      element-loading-text="加载中"
      :data="tableData"
      border
      stripe
      style="width: 95%; margin-left: 2.5%"
      @sort-change="handleSortChange"
    >
      <!-- prop是对应列内容的字段名 width是对应列宽度 sortable是是否排序 label是对应列名称 -->
      <el-table-column
        v-for="(item, index) in tableColumns"
        :key="index"
        :prop="item.param"
        :width="item.width ? item.width : ''"
        :sortable="item.sortable ? true : false"
        :label="item.label"
      >
        <!-- 因为有些参数需要判定比如性别所以判断一下 -->
        <template slot-scope="scope">
          <span v-if="item.render">
            {{ item.render(scope.row) }}
          </span>
          <span v-else>{{ scope.row[item.param] }}</span>
        </template>
      </el-table-column>
      <!-- 是否需要全选， isshow控制-->
      <el-table-column
        v-if="isshow"
        type="selection"
        align="center"
        fixed
      ></el-table-column>
      <!-- 操作 -->
      <el-table-column
        v-if="tableOption.label"
        :label="tableOption.label"
        align="center"
        class-name="small-padding fixed-width"
      >
        <template slot-scope="scope">
          <el-button
            v-for="(item, index) in tableOption.options"
            :key="index"
            :type="item.type"
            :icon="item.icon"
            @click="handleButton(item.methods, scope.row, scope.$index)"
            size="mini"
          >
            {{ item.label }}
          </el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 表格结束 -->
    <!-- 翻页 -->
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="page"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="rows"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    >
    </el-pagination>
  </div>
</template>

<script>
export default {
  props: {
    loading: {
      // 预加载
      type: Boolean,
      default: false,
    },
    tableData: {
      // 表格数据
      type: Array,
      default: () => {
        return [];
      },
    },
    tableColumns: {
      // 表头数据
      type: Array,
      default: () => {
        return [];
      },
    },
    isshow: {
      type: Boolean,
      default: false,
    },
    tableOption: {
      // 表格操作
      type: Object,
      default: () => {
        return {};
      },
    },
    page: {
      // 当前页
      type: Number,
      default: () => {
        return 1;
      },
    },
    rows: {
      // 当前页展示数
      type: Number,
      default: () => {
        return 20;
      },
    },
    total: {
      // 总数
      type: Number,
      default: () => {
        return 0;
      },
    },
  },
  data() {
    return {
      searchVal: "",
    };
  },
  methods: {
    /**刷新页面且清空表单数据 */
    refresh() {
      this.form = { name: "", age: "", sex: "" };
    },
    /**刷新页面且清空表单数据 */
    search() {
      //  this.$emit("查询的方法,自定义", this.form);
    },
    // 切换当前一页展示多少条
    handleSizeChange(val) {
      this.$emit("sizeChange", val);
    },
    // 翻页
    handleCurrentChange(val) {
      this.$emit("pageChange", val);
    },
    // 按钮点击事件
    // methods方法名 row当前点击列数据 index当前点击的index
    handleButton(methods, row, index) {
      this.$emit("clickButton", { methods: methods, row: row, index: index });
    },
    // 点击排序
    handleSortChange(val) {
      this.$emit("sortChange", val);
    },
  },
};
</script>

<style lang="scss" scoped>
.table {
  margin-top: 10px;
}
.el-table {
  margin: 10px 0;
  & td,
  & th {
    text-align: center;
  }
}
.el-pagination {
  text-align: right;
}

// 设置表格行高
.el-table__header tr,
.el-table__header th {
  padding: 0;
  height: 40px;
}
.el-table__body tr,
.el-table__body td {
  padding: 0;
  height: 40px;
}
.searchBox {
  display: flex;
  justify-content: space-between;
  width: 95%;
  margin-left: 2.5%;
  #form {
    display: flex;
    flex: 2;
  }
}
</style>