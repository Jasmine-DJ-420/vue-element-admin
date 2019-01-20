<template>
  <div class="app-container">
    <div class="dashboard-editor-container">
      <el-row :gutter="32">
        <el-col :xs="24" :sm="24" :lg="8">
          <div class="chart-wrapper">
            <span class="table-name">Labeling Data Set</span>
            <bar-chart :chart-data="labeling"/>
          </div>
        </el-col>
        <el-col :xs="24" :sm="24" :lg="8">
          <div class="chart-wrapper">
            <span class="table-name">MySQL Data Set</span>
            <bar-chart :chart-data="mysql"/>
          </div>
        </el-col>
        <el-col :xs="24" :sm="24" :lg="8">
          <div class="chart-wrapper">
            <span class="table-name">MongoDB Data Set</span>
            <bar-chart :chart-data="mongo"/>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>

</template>

<script>
import axios from 'axios'

import LabelingData from './components/LabelingData'
import MysqlData from './components/MysqlData'
import MongoDBData from './components/MongoDBData'
import BarChart from './components/BarChart'

export default {
  name: 'Index',
  components: { LabelingData, MysqlData, MongoDBData, BarChart },
  data() {
    return {
      labeling: [],
      mysql: [],
      mongo: []
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      const path = `http://127.0.0.1:2345/api/dataset`
      axios.get(path)
        .then(({ data }) => {
          this.labeling = data.statistics.labeling
          this.mysql = data.statistics.mysql
          this.mongo = data.statistics.mongodb
        })
        .catch(error => {
          console.log(error)
        })
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
.dashboard-editor-container {
  padding: 32px;
  background-color: rgb(240, 242, 245);
  .chart-wrapper {
    background: #fff;
    padding: 16px 16px 0;
    margin-bottom: 32px;
  }
}
.table-name {
  color: #b5bcc2;
  text-align: center;
}
</style>
