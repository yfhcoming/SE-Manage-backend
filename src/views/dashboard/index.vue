<template>
  <div>
    <el-row :gutter="20" class="mgb20" style="padding-top: 1%;padding-left: 1%;padding-right: 1%">
      <el-col :span="6">
        <el-card shadow="hover" :body-style="{padding: '0px'}">
          <div class="grid-content grid-con-1">
            <div class="grid-cont-right">
              <div class="grid-num">{{userCount}}</div>
              <div>用户总数</div>
            </div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="6">
        <el-card shadow="hover" :body-style="{padding: '0px'}">
          <div class="grid-content grid-con-2">
            <div class="grid-cont-right">
              <div class="grid-num">{{CRSCount}}</div>
              <div>班级总数</div>
            </div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="6">
        <el-card shadow="hover" :body-style="{padding: '0px'}">
          <div class="grid-content grid-con-3">
            <div class="grid-cont-right">
              <div class="grid-num">{{EXPCount}}</div>
              <div>实验数量</div>
            </div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="6">
        <el-card shadow="hover" :body-style="{padding: '0px'}">
          <div class="grid-content grid-con-4">
            <div class="grid-cont-right">
              <div class="grid-num">{{CMTCount}}</div>
              <div>讨论热度</div>
            </div>
          </div>
        </el-card>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="12">
        <h3 style="margin-bottom: 20px;margin-left: 5%">用户性别比例</h3>
        <div class="cav-info" style="background-color: white">
          <ve-pie :data="userSex" :theme="options"></ve-pie>
        </div>
      </el-col>
      <el-col :span="12">
        <h3 style="margin-bottom: 20px;margin-left: 5%">课程实验统计</h3>
        <div class="cav-info" style="background-color: white">
          <ve-histogram :data="tagStyle" :theme="options3"></ve-histogram>
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="12">
        <h3 style="margin: 20px 0;margin-left: 5%">老师性别比例</h3>
        <div class="cav-info" style="background-color: white">
          <ve-pie :data="teaSex" :theme="options1"></ve-pie>
        </div>
      </el-col>
      <el-col :span="12">
        <h3 style="margin: 20px 0;margin-left: 5%">学生性别比例</h3>
        <div class="cav-info" style="background-color: white">
          <ve-pie :data="stuSex" :theme="options2"></ve-pie>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import { mixin } from '@/mixins/index'

export default {
  name: 'Dashboard',
  mixins: [mixin],
  data() {
    return {
      user: [],
      userSex: {
        columns: ['性别', '总数'],
        rows: [
          {'性别': '男', '总数': 0},
          {'性别': '女', '总数': 0}
        ]
      },
      teaSex: {
        columns: ['性别', '总数'],
        rows: [
          {'性别': '男', '总数': 0},
          {'性别': '女', '总数': 0}
        ]
      },
      stuSex: {
        columns: ['性别', '总数'],
        rows: [
          {'性别': '男', '总数': 0},
          {'性别': '女', '总数': 0}
        ]
      },
      country: {
        columns: ['国家', '总数'],
        rows: [
          {'国家': '中国', '总数': 0},
          {'国家': '韩国', '总数': 0},
          {'国家': '意大利', '总数': 0},
          {'国家': '新加坡', '总数': 0},
          {'国家': '美国', '总数': 0},
          {'国家': '马来西亚', '总数': 0},
          {'国家': '西班牙', '总数': 0},
          {'国家': '日本', '总数': 0}
        ]
      },
      options: {
        color: ['#87CEFA', '#FFC0CB']
      },
      options1: {
        color: ['#1E90FF', '#7B68EE']
      },

      options2: {
        color: ['#FEED78', '#FD8A61']
      },
      // options2: {
      //   color: ['#FEED78'],
      //   tooltip: {
      //     trigger: 'axis',
      //     axisPointer: { // 坐标轴指示器，坐标轴触发有效
      //       type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
      //     }
      //   },
      //   grid: {
      //     left: '3%',
      //     right: '4%',
      //     bottom: '3%',
      //     containLabel: true
      //   }
      // },
      options3: {
        color: ['#FD8A61'],
        tooltip: {
          trigger: 'axis',
          axisPointer: { // 坐标轴指示器，坐标轴触发有效
            type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        }
      },
      tagStyle: {
        columns: ['分格', '总数'],
        rows: [
          {'分格': '华语', '总数': 8},
          {'分格': '粤语', '总数': 6},
          {'分格': '欧美', '总数': 2},
          {'分格': '日韩', '总数': 3},
          {'分格': 'BGM', '总数': 9},
          {'分格': '轻音乐', '总数': 5},
          {'分格': '乐器', '总数': 4}
        ]
      },
      userCount: 0,
      CRSCount: 0,
      EXPCount: 0,
      CMTCount: 0,
      tagData:'',
    }
  },
  mounted() {
    this.getUser()
    this.getCRS()
    this.getEXP()
    this.getCMT()
    this.getUserRatio()
    this.getTeaRatio()
    this.getStuRatio()
    this.getCRSALL()
  },
  methods: {
    getUser() {
      var url = "/api/sys/users"
      this.axios
        .get(url, {
          params: {},
          crossDomain: true,
        })
        .then(response => {
          console.log(response);
          this.userCount = response.data.result;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    getUserRatio() {
      var url = "/api/sys/users/sexRatio"
      this.axios
        .get(url, {
          params: {},
          crossDomain: true,
        })
        .then(response => {
          console.log("getUserRatio")
          console.log(response);
          this.userSex.rows[0]['总数'] = response.data.result[0]
          this.userSex.rows[1]['总数'] = response.data.result[1]
        })
        .catch(function (error) {
          console.log(error);
        });
    },

    getTeaRatio() {
      var url = "/api/sys/teas/sexRatio"
      this.axios
        .get(url, {
          params: {},
          crossDomain: true,
        })
        .then(response => {
          console.log("getTeaRatio")
          console.log(response);
          this.teaSex.rows[0]['总数'] = response.data.result[0]
          this.teaSex.rows[1]['总数'] = response.data.result[1]
        })
        .catch(function (error) {
          console.log(error);
        });
    },

    getStuRatio() {
      var url = "/api/sys/stus/sexRatio"
      this.axios
        .get(url, {
          params: {},
          crossDomain: true,
        })
        .then(response => {
          console.log("getTeaRatio")
          console.log(response);
          this.stuSex.rows[0]['总数'] = response.data.result[0]
          this.stuSex.rows[1]['总数'] = response.data.result[1]
        })
        .catch(function (error) {
          console.log(error);
        });
    },

    getCRSALL(){
      var url = "/api/sys//all-course"
      this.axios
        .get(url, {
          params: {},
          crossDomain: true,
        })
        .then(response => {
          console.log("getCRSALL")
          console.log(response);
          this.tagData=response.data.result
          this.setTag()
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    setTag(){
      let count = 0
      for (let item of this.tagData) {
        if (count===7)
        {
          break;
        }
        this.tagStyle.rows[count]['分格'] = item.courseName;
        count++;
      }
    },

    setSex(sex, arr) {
      let count = 0
      for (let item of arr) {
        if (sex === item.sex) {
          count++
        }
      }
      return count
    },
    getCountry(val) {
      for (let item of this.country.rows) {
        if (val.includes(item['国家'])) {
          item['总数']++
          break
        }
      }
    },
    getStyle(val) {
      for (let item of this.songStyle.rows) {
        if (val.includes(item['分格'])) {
          item['总数']++
        }
      }
    },
    getCRS() {

      var url = "/api/sys/courses"
      this.axios
        .get(url, {
          params: {},
          crossDomain: true,
        })
        .then(response => {
          console.log(response);
          this.CRSCount = response.data.result;
        })
        .catch(function (error) {
          console.log(error);
        });
      // HttpManager.getAllSinger().then(res => {
      //   this.singerCount = res.length
      //   this.singerSex.rows[0]['总数'] = this.setSex(1, res)
      //   this.singerSex.rows[1]['总数'] = this.setSex(0, res)
      //   for (let item of res) {
      //     this.getCountry(item.location)
      //   }
      // }).catch(err => {
      //   console.log(err)
      // })
    },
    getEXP() {
      var url = "/api/sys/exps"
      this.axios
        .get(url, {
          params: {},
          crossDomain: true,
        })
        .then(response => {
          console.log(response);
          this.EXPCount = response.data.result;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    getCMT() {
      var url = "/api/sys/comments"
      this.axios
        .get(url, {
          params: {},
          crossDomain: true,
        })
        .then(response => {
          console.log(response);
          this.CMTCount = response.data.result;
        })
        .catch(function (error) {
          console.log(error);
        });
    }
  }
}
</script>

<style>
.grid-content {
  display: flex;
  align-items: center;
  height: 100px;
}

.grid-cont-right {
  flex: 1;
  text-align: center;
  font-size: 14px;
  color: #999;
}

.grid-num {
  font-size: 30px;
  font-weight: bold;
}

.cav-info {
  border-radius: 6px;
  overflow: hidden;
}
</style>
