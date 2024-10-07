<template>
  <div class="dashboard-container">
    <div class="container">
      <div class="tableBar">
        <label style="margin-right: 5px;">员工姓名:</label>
        <el-input placeholder="请输入员工姓名" style="width:12%;" />
        <el-button type="primary" style="margin-left: 5px;" @click="pageQuery">搜索</el-button>
        <el-button type="primary" style="float: right;">+添加员工</el-button>
      </div>
    </div>

  </div>
</template>
<script lang="ts">
import { getEmployeeList } from '@/api/employee'
export default  {
  methods: {
    //分页查询
    pageQuery(){
      getEmployeeList({name:'admin', page: 1, pageSize: 10})
    }
  },
}
</script>

<style lang="scss" scoped>
.disabled-text {
  color: #bac0cd !important;
}
</style>
