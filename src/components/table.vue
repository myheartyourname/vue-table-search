<template>
<div>
  <el-input v-model="search" placeholder="请输入内容"></el-input>
    <el-table
    :data="tables.slice((pageIndex-1)*pageSize,pageIndex*pageSize)"
    style="width: 100%">
    <el-table-column
      label="日期"
      width="180">
      <template slot-scope="scope">
        <i class="el-icon-time"></i>
        <span style="margin-left: 10px" v-html="format(scope.row.date)"></span>
      </template>
    </el-table-column>
    <el-table-column
      label="姓名"
      width="180">
      <template slot-scope="scope">
        <el-popover trigger="hover" placement="top">
          <p>姓名: {{ scope.row.name }}</p>
          <p>住址: {{ scope.row.address }}</p>
          <div slot="reference" class="name-wrapper">
            <el-tag size="medium" v-html="format(scope.row.name)"></el-tag>
          </div>
        </el-popover>
      </template>
    </el-table-column>
    <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button
          size="mini"
          @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
        <el-button
          size="mini"
          type="danger"
          @click="handleDelete(scope.$index, scope.row)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
      <div class="block">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="pageIndex"
          :page-sizes="[2, 3, 4, 5]"
          :page-size="pageSize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>
    </div>
</template>

<script>
import moment from 'moment'
export default {
  data () {
    return {
      pageSize: 3, // 每页展示数量
      pageIndex: 1, // 页码，默认第一页
      search: '',  // 搜索词
      // 列表数据，可使用axios请求接口数据
      tableData: [{
        date: '2016-05-02',
        name: '张三',
        address: '郑州市金水区文化路'
      }, {
        date: '2016-05-04',
        name: '李四',
        address: '上海市普陀区金沙江路 1517 弄'
      }, {
        date: '2016-05-01',
        name: '王伯虎',
        address: '北京市朝阳区长安街'
      }, {
        date: '2016-05-03',
        name: '赵又廷',
        address: '西安市'
      }]
    }
  },

  mounted () {
  },

  methods: {
    // 前端过滤
    format (val) {
      val = val.toString()
      if (val.indexOf(this.search) !== -1 && this.search !== '') {
        return val.replace(this.search, '<font color="red">' + this.search + '</font>')
      } else {
        return val
      }
    },
    dateFormat (val) {
      return moment(val).format('YYYY-MM-DD')
    },
    handleSizeChange (val) {
      this.pageSize = val
    },
    handleCurrentChange (val) {
      this.pageIndex = val
    },
    siteTableHeader () {
      return 'sws-table-header'
    },
    tableRowClassName ({ row, rowIndex }) {
      console.log('row', row);
      if (rowIndex % 2) {
        return 'warning-row'
      } else {
        return 'success-row'
      }
    }
  },
  computed: {
    // 前端过滤
    tables () {
      const search = this.search
      if (search) {
        return this.tableData.filter(dataNews => {
          return Object.keys(dataNews).some(key => {
            return String(dataNews[key]).toLowerCase().indexOf(search) > -1
          })
        })
      }
      return this.tableData
    },
    // 总条数
    total () {
      return this.tables.length
    }
  },
  watch: {
   // 检测表格数据过滤变化，自动跳到第一页
    tables () {
      this.pageIndex = 1
    }
  }
}
</script>
