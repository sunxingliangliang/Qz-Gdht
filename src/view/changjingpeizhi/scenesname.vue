<template>
  <div>
    <sx></sx>
    <el-row :class="$style.f_row">
      <div :class="$style.f_da">
        <el-card shadow="hover">
          <span :class="$style.f_sl">{{Scenesname}}</span>
          <p>场景名称</p>
        </el-card>
      </div>
    </el-row>
    <!-- 操作 -->
    <div style="text-align: left; margin-bottom: 20px;">
      <el-input placeholder="请输入任务名称" v-model="input" clearable style=" width: calc(100% - 80%);float: left;"></el-input>
      <el-button type="success" size="medium" @click="search" icon="el-icon-search" style="margin-left:20px">搜索</el-button>
      <el-button size="medium" type="primary" @click="zengjia">增加</el-button>
    </div>
    <!-- 表格 -->
    <div>
      <el-table :data="tableData" border style="width: 100%" v-loading="loading" element-loading-text="拼命加载中" element-loading-spinner="el-icon-loading">
        <el-table-column prop="scencesName" label="名称"></el-table-column>
        <el-table-column label="地点">
          <template slot-scope="scope">
            <el-button size="mini" @click="examine(scope.$index, scope.row)">查看</el-button>
          </template>
        </el-table-column>
        <el-table-column prop="address" label="数据类型">
          <template slot-scope="scope">
            <span v-if="scope.row.type===1" >区域数据</span>
            <span v-if="scope.row.type===2" >行业数据</span>
          </template>
        </el-table-column>
        <el-table-column prop="address" label="区域/行业类型">
          <template slot-scope="scope">
            <span>{{scope.row.rAndIparent}}\{{scope.row.rAndI}}</span>
          </template>
        </el-table-column>
        <el-table-column prop="address" label="场景类型">
          <template slot-scope="scope">
            <span>{{scope.row.scenceTypeParent}}\{{scope.row.scenceType}}</span>
          </template>
        </el-table-column>
        <el-table-column label="人群标签">
          <template slot-scope="scope">
            <span v-for="(item,i) in scope.row.crowds" :key="i">{{item.name}},</span>
          </template>
        </el-table-column>
         <el-table-column label="去重方式">
          <template slot-scope="scope">
            <span v-if="scope.row.deduplication===0">一年去重</span>
            <span v-if="scope.row.deduplication===1">一天去重</span>
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
            <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <!-- 分页 -->
    <div>
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage4"
        :page-sizes="[10, 20, 30, 40]"
        :page-size="100"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </div>
    <!-- 地理围栏 -->
    <div class="f_dg" :class="$style.f_map" v-show="Fence">
      <div :class="$style.f_backdrop" @click="stop"></div>
      <!-- <div id="allmap" style="height:400px;"></div> -->
      <amap-weilan :simple="true" ref="amapWeilan"></amap-weilan>
    </div>
  </div>
</template>

<script>
import sx from '../components/sxbtn'
import AmapWeilan from "./AMapWeilan";
export default {
  components: {
    sx,
    AmapWeilan
  },
  data () {
    return {
      total: null,
      currentPage4: 1,
      tableData: [],
      Scenesname: '',
      Fence: false,
      sizes:10,
      pages:0,
      input:'',
      loading:true
    }
  },
  mounted () {
    this.getList()
    this.getCount();
  },
  methods: {
    search(){
      this.$http.get(`modules/scencesName`, {
        params: {
          size: this.sizes,
          page: this.pages,
          name:this.input
        }
      }).then(res => {
        var { code, data } = res.data
        if (code === 1000) {
          this.loading = false
          this.tableData = data.content
          this.total = data.total
        } else if (code == 2001) {
          this.$message.error(res.data.message);
          window.sessionStorage.clear();
          window.localStorage.clear();
          this.$router.push('/')
        } else {
          this.$message.error(res.data.message);
        }
      }).catch((err) => {
        console.log('错误信息' + err)
      })
    },
    getList () {
      this.$http.get(`modules/scencesName`, {
        params: {
          size: this.sizes,
          page: this.pages,
        }
      }).then(res => {
        var { code, data } = res.data
        if (code === 1000) {
          this.loading = false
          this.tableData = data.content
          this.total = data.total
        } else if (code == 2001) {
          this.$message.error(res.data.message);
          window.sessionStorage.clear();
          window.localStorage.clear();
          this.$router.push('/')
        } else {
          this.$message.error(res.data.message);
        }
      }).catch((err) => {
        console.log('错误信息' + err)
      })
    },
    getCount () {
      this.$http.get('modules/scencesName/scencesCount').then(res => {
        var { code, data } = res.data
        if (code === 1000) {
          // console.log(data)
          this.Scenesname = data
        } else if (code == 2001) {
          this.$message.error(res.data.message);
          window.sessionStorage.clear();
          window.localStorage.clear();
          this.$router.push('/')
        } else {
          this.$message.error(res.data.message);
        }
      }).catch((err) => {
        console.log('错误信息' + err)
      })
    },
    examine (index, row) {
      this.Fence = true
      var str = row.lbs
      this.$refs.amapWeilan.loadLbs(str);
    },
    stop () {
      this.Fence = false
    },
    shutdown () {
      this.Fence = false
    },
    handleSizeChange (val) {
     this.sizes=val
     this.$http.get(`modules/scencesName`, {
        params: {
          size: val,
          page: this.pages,
        }
      }).then(res => {
        var { code, data } = res.data
        if (code === 1000) {
          this.tableData = data.content
          this.total = data.total
        } else if (code == 2001) {
          this.$message.error(res.data.message);
          window.sessionStorage.clear();
          window.localStorage.clear();
          this.$router.push('/')
        } else {
          this.$message.error(res.data.message);
        }
      }).catch((err) => {
        console.log('错误信息' + err)
      })
    },
    handleCurrentChange (val) {
      this.pages=val
      this.$http.get(`modules/scencesName`, {
        params: {
          size: this.sizes,
          page: val - 1,
        }
      }).then(res => {
        var { code, data } = res.data
        if (code === 1000) {
          this.tableData = data.content
          this.total = data.total
        } else if (code == 2001) {
          this.$message.error(res.data.message);
          window.sessionStorage.clear();
          window.localStorage.clear();
          this.$router.push('/')
        } else {
          this.$message.error(res.data.message);
        }
      }).catch((err) => {
        console.log('错误信息' + err)
      })
    },
    handleEdit (idnex, row) {
      // this.obj = {
      //   title: '场景编辑',
      //   name: '/index/editScenes.vue',
      // }
      // this.$store.commit('mypush', this.obj)
      this.$router.push({ path: '/index/editScenes.vue', query: { id: row.id } })
    },
    handleDelete (index, row) {
      this.$http.delete(`modules/scencesName/${row.id}`).then(res => {
        var { code, data } = res.data
        if (code === 1000) {
          this.getList()
        } else if (code == 2001) {
          this.$message.error(res.data.message);
          window.sessionStorage.clear();
          window.localStorage.clear();
          this.$router.push('/')
        } else {
          this.$message.error(res.data.message);
        }
      }).catch((err) => {
        console.log('错误信息' + err)
      })
    },
    zengjia () {
      // this.obj = {
      //   title: '场景创建',
      //   name: '/index/newadd.vue',
      // }
      // this.$store.commit('mypush', this.obj)
      this.$router.push('/index/newadd.vue')
    }
  }
}
</script>

<style lang='scss' module>
.f_row {
  margin-bottom: 20px;
  .f_da {
    width: 330px;
    margin: 0 auto;
  }
  .f_sheng {
    margin: 0 24px;
  }
}
.f_sl {
  margin-top: 0;
  font-family: "MicrosoftYaHei", "微软雅黑";
  font-size: 28px;
}
.f_backdrop {
  position: fixed;
  width: 100%;
  height: 100%;
  background: #000;
  z-index: 0;
  top: 0;
  left: 0;
  opacity: 0.7;
}
.f_map {
  width: 700px;
  height: 400px;
  position: fixed;
  top: 20%;
  background: #fff;
  z-index: 3;
  left: 30%;
}
</style>
