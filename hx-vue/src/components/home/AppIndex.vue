<template>
  <div>
    <search-bar @onSearch="searchResult" ref="SearchBar"></search-bar>
    ss Hello World!aaa Vue.use(ElementUI)
    <div class="project_table">
      <el-table :data="tableDataTest" style="width:100%" stripe  :default-sort = "{prop: 'update_time', order: 'descending'}">
        <el-table-column type="expand">
          <template slot-scope="props">
            <el-form label-position="left" inline class="demo-table-expand">
              <el-form-item label="项目名称" class="biu">
                <span>{{ props.row.name }}</span>
                <xm-tag :type="FlowStatusRules[0]">ddddd</xm-tag>
              </el-form-item>
            </el-form>
          </template>
        </el-table-column>
        <el-table-column label="项目id" prop="id"></el-table-column>
        <el-table-column label="项目名称" prop="name"></el-table-column>
        <el-table-column label="项目状态" prop="status">
          <template slot-scope="props">
            <xm-tag :type="FlowStatusRules[props.row.status]">
              {{ FLOWS_STATUS[props.row.status] }}
            </xm-tag>
          </template>
        </el-table-column>
        <el-table-column sortable label="更新时间" prop="update_time"></el-table-column>
      </el-table>
    </div>
  </div>
</template>
<script>
import { FlowStatusRules } from './rule/data-config'
import SearchBar from './home_component/SearchBar'
import XmTag from '../tag'
export default {
  components: {
    'search-bar': SearchBar,
    'xm-tag': XmTag
  },
  name: 'AppIndex',
  data () {
    return {
      FlowStatusRules,
      FLOWS_STATUS: ['已启用', '进行中', '已失效', '已删除'],
      projects: [
        {
          id: '',
          name: '',
          status: '',
          update_time: ''
        }
      ],
      tableDataTest: [
        {
          id: '1',
          name: 'biu',
          status: 0,
          update_time: '2016-05-04'
        },
        {
          id: '2',
          name: 'biu',
          status: 1,
          update_time: '2016-05-02'
        },
        {
          id: '3',
          name: 'biu2',
          status: 3,
          update_time: '2017-05-02'
        }
      ]
    }
  },
  methods: {
    searchResult () {
      var _this = this
      this.$axios
        .get('/search?keywords=' + this.$refs.searchBar.keywords, {})
        .then(resp => {
          if (resp && resp.status === 200) {
            _this.projects = resp.data
          }
        })
    }
  }
}
</script>
<style lang="scss" scoped>
.project_table {
  padding-top: 0;
  margin: 10px 15%;
  position: relative;
}
.demo-table-expand {
  font-size: 0;
}
.demo-table-expand label {
  width: 90px;
  color: #99a9bf;
}
.demo-table-expand .el-form-item {
  margin-right: 0;
  margin-bottom: 0;
  width: 50%;
  color: red;
}
</style>
