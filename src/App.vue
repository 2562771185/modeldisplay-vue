<template>
  <div class="app-container">
    <hr>
    <div style="margin-bottom: 10px">
      <span>项目名称</span>
      <el-input v-model="xmmc" placeholder="输入项目名称"
                style="width: 200px;margin: auto 25px auto 10px;"
                prefix-icon="el-icon-search"
                :clearable="true"
      ></el-input>
      <span>项目业主</span>
      <el-input v-model="xmyz" placeholder="输入项目业主"
                style="width: 200px;margin: auto 25px auto 5px;"
                prefix-icon="el-icon-user"
                :clearable="true"
      ></el-input>
      <span>责任单位</span>
      <el-input v-model="zrdw" placeholder="输入责任单位"
                style="width: 200px;margin: auto 25px auto 10px;"
                prefix-icon="el-icon-s-home"
                :clearable="true"
      ></el-input>
      <span>项目层级</span>
      <el-input v-model="xmcj" placeholder="输入项目层级"
                style="width: 200px;margin: auto 25px auto 10px;"
                prefix-icon="el-icon-finished"
                :clearable="true"
      ></el-input>
      <el-button type="success" style="margin: 5px" @click="searchKeyWord">查询</el-button>
      <el-button type="info" style="margin: 5px" @click="clearSearch">重置</el-button>
    </div>
    <div class="box">
      <h1 style="margin: auto" v-if="tableData.length <= 0">暂无数据</h1>
      <div v-if="tableData.length > 0" v-for="item of tableData" :key="item.id">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span style="font-size: 15px;font-weight: bold">{{ item.xmmc }}</span>
            <el-button style="float: right; padding: 3px 0" type="text" @click="getDetails(item.id)">查看详情</el-button>
          </div>
          <div class="text item">
            项目代码：{{ item.xmdm }}
          </div>
          <div class="text item">
            总投资额： {{ item.ztz }}
          </div>
          <div class="text item">
            项目状态：
            <el-tag
                :type="tagType(item.jsxz)"
            >{{ item.jsxz }}
            </el-tag>
          </div>
          <!--                动画-->
<!--          <div class="text item">-->
<!--            &lt;!&ndash;            <div class="box-dh"></div>&ndash;&gt;-->
<!--          </div>-->
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
      axios.get(this.actionHost + '/yc/sso/xmxx/page?pageNum=' + num + "&pageSize=" + size +
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
    },
    tagType(name) {
      switch (name) {
        case
        '续建类':
          return 'success';
        case
        '竣工投产类':
          return 'info';
        case
        '预备类':
          return 'warning';
        case
        '新开工类':
          return 'danger';
        default:
          return '';
      }
    },
    // getDetails(id) {
    //   console.log(id)
    //   let url = global.host + '/yc/formDesign/index.html#/formView/2d4cf21fcdeab8e0b732f2a562c1f316'
    //   location.href = url
    // },
    getDetails(id) {
      console.log(id)
      let url = global.host + '/yc/formDesign/index.html#/formView/2d4cf21fcdeab8e0b732f2a562c1f316?businessId=' + id;
      location.href = url
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
  height: auto;
  width: 100%;
  max-width: 1400px;
  overflow: auto;
  display: flex;
  flex-wrap: wrap;
  background-color: rgba(5, 55, 91, 0.39);
}

.box-card {
  width: 300px;
  height: 240px;
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
