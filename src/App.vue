<template>

  <div class="app-container">
    <h1>数据模块化展示</h1>
    <hr>
    <div>
      <span>项目名称</span>
      <el-input v-model="xmmc" placeholder="输入项目名称"
                style="width: 220px;margin: auto 25px auto 10px;"
                prefix-icon="el-icon-search"
                :clearable="true"
      ></el-input>
      <span>项目业主</span>
      <el-input v-model="xmyz" placeholder="输入项目业主"
                style="width: 220px;margin: auto 25px auto 5px;"
                prefix-icon="el-icon-user"
                :clearable="true"
      ></el-input>
      <span>责任单位</span>
      <el-input v-model="zrdw" placeholder="输入责任单位"
                style="width: 220px;margin: auto 25px auto 10px;"
                prefix-icon="el-icon-s-home"
                :clearable="true"
      ></el-input>
      <span>项目层级</span>
      <el-input v-model="xmcj" placeholder="输入项目层级"
                style="width: 220px;margin: auto 25px auto 10px;"
                prefix-icon="el-icon-finished"
                :clearable="true"
      ></el-input>
      <el-button type="success" style="margin: 10px" @click="searchKeyWord">查询</el-button>
      <el-button type="info" style="margin: 10px" @click="clearSearch">重置</el-button>
    </div>
    <div class="box">
      <div v-for="item of tableData" :key="item.name">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span style="font-size: 15px;font-weight: bold">{{ item.xmmc }}  <el-tag size="mini" type="danger">{{
                item.name
              }}</el-tag></span>
            <el-button style="float: right; padding: 3px 0" type="text">查看详情</el-button>
          </div>
          <div class="text item">
            项目代码：{{ item.xmdm }}
          </div>
          <div class="text item">
            总投资额： {{ item.ztz }}
          </div>
          <div class="text item">
            <!--                  todo 根据状态调整样式-->
            项目状态：
            <el-tag>{{ item.jsxz }}</el-tag>
          </div>
          <!--                动画-->
          <div class="text item">
            <div class="box-dh"></div>
          </div>
        </el-card>
      </div>
    </div>
    <div class="page">
      <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="pageNum"
          :page-sizes="[8, 16, 32, 64]"
          :page-size="pageSize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total"
      >
      </el-pagination>
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
      pageSize: 8,
      total: 100,
      xmmc: "",
      xmyz: "",
      zrdw: "",
      xmcj: "",
    }
  },
  mounted() {
  },
  created() {
    var token = Cookies.get("access_token");
    this.mytoken = {Authorization: token}
    this.getPage(1, 8)
  },
  watch: {},
  methods: {
    getPage(num, size) {
      axios.get(this.actionHost + '/xmxx/page?pageNum=' + num + "&pageSize=" + size +
          "&xmmc=" + this.xmmc + "&xmyz=" + this.xmyz + "&zrdw=" + this.zrdw + "&xmcj=" + this.xmcj).then(res => {
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
    searchKeyWord() {
      this.getPage(this.pageNum, this.pageSize)
    },
    clearSearch() {
      this.xmmc = ""
      this.xmcj = ""
      this.zrdw = ""
      this.xmyz = ""
      this.pageNum = 1
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

.text {
  font-size: 14px;
}

.item {
  margin-bottom: 18px;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}

.clearfix:after {
  clear: both
}

.box {
  text-align: left;
  margin: auto;
  height: 550px;
  width: 100%;
  position: relative;
  overflow: auto;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  background-color: rgba(5, 55, 91, 0.39);
}

.box-card {
  width: 380px;
  margin: 20px;
}

.box-dh {
  width: 130px;
  height: 130px;
  border: 1px lightcoral solid;
  border-radius: 50%;
  float: right;
  position: relative;
  margin-top: -130px;
}

.page {
  margin: 10px;
}
</style>
