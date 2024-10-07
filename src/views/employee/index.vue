<template>
  <div class="dashboard-container">
    <div class="container">
      <div class="tableBar">
        <label style="margin-right: 5px;">员工姓名:</label>
        <el-input placeholder="请输入员工姓名" style="width:12%;" v-model="name" />
        <el-button type="primary" style="margin-left: 5px;" @click="pageQuery">搜索</el-button>
        <el-button type="primary" style="float: right;">+添加员工</el-button>
      </div>
      <div class="table">
        <el-table :data="this.records" stripe style="width: 100%">
          <el-table-column prop="username" label="员工姓名" width="180" />
          <el-table-column prop="name" label="账号" width="180" />
          <el-table-column prop="phone" label="手机号" />
          <el-table-column prop="status" label="账号状态" >
            <template slot-scope="scope">
              {{ scope.row.status === 0 ? '禁用' : '启用' }}
            </template>
          </el-table-column>
          <el-table-column prop="updateTime" label="最后操作时间" />
          <el-table-column label="操作">
            <template slot-scope="scope">
              <el-button type="text">修改</el-button>
              <el-button type="text">{{ scope.row.status === 1 ? '禁用' : '启用' }}</el-button>
              <el-button type="text">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-pagination
          class="pageList"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page.sync="this.page"
          :page-sizes="[10, 15, 20, 25]"
          :page-size="this.pageSize"
          layout="total, prev, pager, next, sizes"
          :total="this.total">
        </el-pagination>
      </div>
    </div>

  </div>
</template>
<script lang="ts">
import { getEmployeeList } from '@/api/employee'
export default  {
  data(){
    return {
      name: '', // 员工姓名
      page: 1,  // 页码
      pageSize: 10, // 每页记录数
      total: 0, // 总记录数
      records: [] // 数据集合
    }
  },
  created(){
    this.pageQuery()
  },
  methods: {
    //分页查询
    pageQuery(){
      const params = {
        name: this.name,
        page: this.page,
        pageSize: this.pageSize 
      }
      getEmployeeList(params).then(res => {
        if(res.data.code === 1){
          this.total = res.data.data.total
          this.records = res.data.data.records
        }
      }).catch(error => {
        this.$message.error("something went wrong:" + error.message)
      })
    },
    // pagesize变化时
    handleSizeChange(pageSize){
      this.pageSize = pageSize
      this.pageQuery()
    },
    // page变化时
    handleCurrentChange(page){
      this.page = page
      this.pageQuery()
    },
  },
}
</script>

<style lang="scss" scoped>
.disabled-text {
  color: #bac0cd !important;
}
</style>
