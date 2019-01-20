<template>
  <el-table :data="list" stripe="" style="width: 100%;padding-top: 15px;" max-height="398">
    <el-table-column type="index" width="70" align="center"/>
    <el-table-column label="User name" width="200" align="center">
      <template slot-scope="scope">
        <router-link :to="'/sentiment/developer/'+owner+'/'+project+'/'+scope.row[0]">
          {{ scope.row[0] }}
        </router-link>
      </template>
    </el-table-column>
    <el-table-column label="Rank Score" width="200" align="center">
      <template slot-scope="scope">
        {{ scope.row[1] }}
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
export default {
  props: {
    list: {
      type: Object,
      required: true
    },
    owner: {
      type: String,
      required: true
    },
    project: {
      type: String,
      required: true
    }
  }
}
</script>

<style>
  .el-table th{
    background: #e2e4e7;
  }
</style>
