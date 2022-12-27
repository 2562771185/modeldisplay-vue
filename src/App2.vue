<template>

  <div class="app-container">
    <h1>AI预警数据（AI系统通过接口推送写入）</h1>
    <el-input v-model="search" placeholder="输入身份证或者姓名查询"
              style="float: left;margin: 10px;width: 220px;"
              prefix-icon="el-icon-search"
              :clearable="true"
              @clear="this.clearSearch"
              @change="this.searchKeyWord"
    ></el-input>
    <el-button type="success" style="float: left;margin: 10px" @click="searchKeyWord">搜索</el-button>
    <el-button style="float: left;margin-top: 10px" @click="clearSearch()">重置</el-button>

    <el-button type="success" style="float: right;margin: 10px" @click="exportAllPageData">导出全部数据</el-button>
    <el-button type="primary" style="float: right;margin: 10px" @click="exportCurrentPageData">导出当前页</el-button>
    <div class="box">
      <el-table
          :data="tableData"
          :highlight-current-row="true"
          border
          stripe
          fit
          height="600"
          max-height="600"
          style="width: 100%">
        <el-table-column
            fixed="left"
            prop="bh"
            label="编号"
            width="100">
          <template slot-scope="scope">
            <i class="el-icon-medal"></i>
            <span style="margin-left: 10px">{{ scope.row.bh }}</span>
          </template>
        </el-table-column>
        <el-table-column
            fixed="left"
            prop="xm"
            label="姓名"
            width="120">
          <template slot-scope="scope">
            <el-tag> {{ scope.row.xm }}</el-tag>
          </template>
        </el-table-column>
        <el-table-column
            prop="sfzh"
            label="身份证号码"
            width="200">
        </el-table-column>
        <el-table-column
            prop="glbm"
            label="关联部门"
            width="100">
        </el-table-column>
        <el-table-column
            prop="sznf"
            label="所属年份"
            width="100">
        </el-table-column>
        <el-table-column
            prop="szyf"
            label="所属月份"
            width="100">
        </el-table-column>
        <el-table-column
            prop="sfsw"
            label="是否死亡"
            width="100">
        </el-table-column>
        <el-table-column
            prop="swsj"
            label="死亡时间"
            width="120">
        </el-table-column>
        <el-table-column
            prop="swxxxq"
            label="死亡信息详情"
            width="300">
        </el-table-column>
        <!--        退役-->
        <el-table-column label="退役">
          <el-table-column
              prop="rylb"
              label="人员类别"
              width="120">
          </el-table-column>
          <el-table-column
              prop="dxzt"
              label="对象状态"
              width="120">
          </el-table-column>
          <el-table-column
              prop="twsj"
              label="退伍时间"
              width="120">
          </el-table-column>
          <el-table-column
              prop="rwsj"
              label="入伍时间"
              width="120">
          </el-table-column>
        </el-table-column>
        <!--        残联-->
        <el-table-column label="残联">
          <el-table-column
              prop="cjlb"
              label="残疾类别"
              width="120">
          </el-table-column>
          <el-table-column
              prop="cjxq"
              label="残疾详情"
              width="120">
          </el-table-column>
          <el-table-column
              prop="cjdj"
              label="残疾等级"
              width="120">
          </el-table-column>
          <el-table-column
              prop="czzt"
              label="持证状态"
              width="120">
          </el-table-column>
        </el-table-column>
        <!--        民政局-->
        <el-table-column label="民政局">
          <el-table-column
              prop="hbzlx"
              label="基本生活保障类型"
              width="140">
          </el-table-column>
          <el-table-column
              prop="city"
              label="儿童保障类型"
              width="120">
          </el-table-column>
          <el-table-column
              prop="cjrbzlxa"
              label="残疾人保障类型1"
              width="130">
          </el-table-column>
          <el-table-column
              prop="cjrbzlxb"
              label="残疾人保障类型2"
              width="130">
          </el-table-column>
          <el-table-column
              prop="glbzlx"
              label="高龄保障类型"
              width="120">
          </el-table-column>
        </el-table-column>
        <!--        乡村振兴-->
        <el-table-column label="乡村振兴">
          <el-table-column
              prop="hlx"
              label="户类型"
              width="120">
          </el-table-column>
          <el-table-column
              prop="jcdxlx"
              label="监测对象类型"
              width="120">
          </el-table-column>
          <el-table-column
              prop="fxsfxc"
              label="风险是否消除"
              width="120">
          </el-table-column>
        </el-table-column>
        <!--        医保局-->
        <el-table-column label="医保局">
          <el-table-column
              prop="mbkbz"
              label="慢病卡病种"
              width="120">
          </el-table-column>
          <el-table-column
              prop="grzfje"
              label="个人自付金额"
              width="120">
          </el-table-column>
          <el-table-column
              prop="sfzzcb"
              label="是否资助参保"
              width="120">
          </el-table-column>
        </el-table-column>
        <!--        人社局-->
        <el-table-column label="人社局">
          <el-table-column
              prop="cbzt"
              label="参保状态"
              width="120">
          </el-table-column>
          <el-table-column
              prop="ydyje"
              label="月待遇金额"
              width="120">
          </el-table-column>
          <el-table-column
              prop="jydw"
              label="就业单位"
              width="120">
          </el-table-column>
          <el-table-column
              prop="btlx"
              label="补贴类型"
              width="120">
          </el-table-column>
          <el-table-column
              prop="btje"
              label="补贴金额"
              width="120">
          </el-table-column>
          <el-table-column
              prop="pxrq"
              label="培训时间"
              width="120">
          </el-table-column>
          <el-table-column
              prop="pxgz"
              label="培训工种"
              width="120">
          </el-table-column>
        </el-table-column>
      </el-table>
      <div class="block">
        <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="pageNum"
            :page-sizes="[10, 30, 50, 100]"
            :page-size="pageSize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
        >
        </el-pagination>
      </div>
    </div>
  </div>
</template>


<script>
import axios from 'axios'
import global from "@/common/Global";
import Cookies from 'js-cookie'

export default {
  name: 'MulTable',
  data() {
    return {
      tableData: [],
      actionHost: global.host,
      msg: "",
      mytoken: null,
      pageNum: 1,
      pageSize: 10,
      total: 100,
      search: "",
    }
  },
  mounted() {
  },
  created() {
    var token = Cookies.get("access_token");
    this.mytoken = {Authorization: token}
    this.getPage(0, 10)
  },
  watch: {},
  methods: {
    getPage(num, size) {
      var key = this.search.trim();
      axios.get(this.actionHost + '/aipage?pageNum=' + num + "&pageSize=" + size + "&key=" + key).then(res => {
        this.tableData = res.data.result.list
        this.total = res.data.result.total
      })
    },
    handleCurrentChange(val) {
      this.pageNum = val
      // console.log(`当前页: ${val}`);
      this.getPage(this.pageNum, this.pageSize)
    },
    handleSizeChange(val) {
      // console.log(`每页 ${val} 条`);
      this.pageSize = val
      this.pageNum = 1
      this.getPage(this.pageNum, this.pageSize)
    },
    exportCurrentPageData() {
      if (this.tableData.length <= 0){
        this.$message({
          type: 'warning',
          message: '无数据,无法导出!'
        });
        return
      }
      var msg = "确认导出本页数据吗?"
      var key = this.search.trim();
      if (key != null && key != "") {
        msg = msg + '根据关键字：' + key
      }
      this.$confirm(msg, '提示', {
        confirmButtonText: '确定导出',
        cancelButtonText: '取消导出',
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          message: '导出成功!'
        });
        const a = document.createElement('a')
        a.setAttribute('download', name)
        // a.setAttribute('target', '_blank')
        a.setAttribute('href', global.host + '/exportCurPageExcel?pageNum=' + this.pageNum + "&pageSize=" + this.pageSize + "&key=" + this.search)
        a.click()
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消导出'
        });
      });

    },
    exportAllPageData() {
      if (this.tableData.length <= 0){
        this.$message({
          type: 'warning',
          message: '无数据,无法导出!'
        });
        return
      }
      var msg = "确认导出全部数据吗?"
      var key = this.search.trim();
      if (key != null && key != "") {
        msg = msg + ' 根据关键字：' + key
      }
      this.$confirm(msg, '提示', {
        confirmButtonText: '确定导出',
        cancelButtonText: '取消导出',
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          message: '导出成功!'
        });
        const a = document.createElement('a')
        a.setAttribute('download', name)
        a.setAttribute('href', global.host + '/exportAllPageExcel?key=' + this.search)
        a.click()
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消导出'
        });
      });

    },
    searchKeyWord() {
      var key = this.search.trim();
      if (key != null && key != "") {
        this.getPage(this.pageNum, this.pageSize)
      }
    },
    clearSearch() {
      this.pageNum = 1
      this.search = ""
      this.getPage(1, this.pageSize)
    }
  }
}
</script>
<style>
.app-container {
  text-align: center; /*让div内部文字居中*/
  background-color: #fff;
  border-radius: 20px;
  width: 100%;
  height: 100%;
  margin: auto;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)
}

</style>
