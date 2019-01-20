<template>
  <div class="app-container">
    <div>
      <ProjectOwnerOption v-model="ownername"/>
      <ProjectnameOption v-model="projectname"/>
      <router-link :to="'/sentiment/'+ownername+'/'+projectname">
        <el-button :loading="searchLoading" style="margin:0 0 20px 20px;" type="primary" icon="el-icon-search">{{ $t('sentiment.search') }}</el-button>
      </router-link>
    </div>

    <div>
      <DevelopernameOption v-model="developername"/>
      <router-link :to="'/sentiment/developer/'+curOwnername+'/'+curProjectname+'/'+developername">
        <el-button style="margin:0 0 20px 20px;" type="primary" icon="el-icon-search">{{ $t('sentiment.searchdeveloper') }}</el-button>
      </router-link>
    </div>

    <div class="dashboard-editor-container">
      <panel-group :cur-ownername="curOwnername" :cur-projectname="curProjectname" :issue-num="issuenum" :contributor-num="contributorsnum"/>

      <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
        <span class="table-name">Sentiment Timeline</span>
        <line-chart :chart-data="lineChartData"/>
      </el-row>

      <el-row :gutter="32">
        <el-col :xs="24" :sm="24" :lg="8">
          <div class="chart-wrapper">
            <span class="table-name">Sentiment Proportion</span>
            <pie-chart :chart-data="pieChartData"/>
          </div>
        </el-col>
        <el-col :xs="24" :sm="24" :lg="8">
          <div class="chart-wrapper" style="height: 433px">
            <span class="table-name">Contributors</span>
            <contributor-table :list="contributors" :owner="curOwnername" :project="curProjectname"/>
          </div>
        </el-col>
        <el-col :xs="24" :sm="24" :lg="8">
          <div class="chart-wrapper" style="height: 433px">
            <span class="table-name">Potential Contributors</span>
            <recommend-table :list="recommend" :owner="curOwnername" :project="curProjectname"/>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>

</template>

<script>
import axios from 'axios'

// options components
import ProjectnameOption from './components/ProjectnameOption'
import ProjectOwnerOption from './components/ProjectOwnerOption'
import DevelopernameOption from './components/DevelopernameOption'
import PanelGroup from './components/PanelGroup'
import LineChart from './components/LineChart'
import ContributorTable from './components/ContributorTable'
import RecommendTable from './components/RecommendTable'
import PieChart from './components/PieChart'

var defaultData = {
  ownername: 'vim',
  projectname: 'vim'
}

export default {
  name: 'Index',
  components: { ProjectOwnerOption, ProjectnameOption, DevelopernameOption, PanelGroup, LineChart, PieChart, ContributorTable, RecommendTable },
  data() {
    return {
      searchLoading: false,
      ownername: '',
      projectname: '',
      issuenum: 0,
      contributorsnum: 0,
      developername: '',
      curOwnername: '',
      curProjectname: '',
      lineChartData: {
        axis: [],
        positive: [],
        negative: []
      },
      pieChartData: [],
      contributors: [],
      recommend: []
    }
  },
  created() {
    this.ownername = this.$route.params && this.$route.params.owner
    this.projectname = this.$route.params && this.$route.params.project
    this.fetchData(this.ownername, this.projectname)
  },
  methods: {
    fetchData(ownername, projectname) {
      const sentiment_path = `http://127.0.0.1:2345/api/` + ownername + `/` + projectname + `/emotion_timing`
      const information_path = `http://127.0.0.1:2345/api/` + ownername + `/` + projectname + `/information`
      const contributor_path = `http://127.0.0.1:2345/api/` + ownername + `/` + projectname + `/contributor`
      this.curOwnername = ownername
      this.curProjectname = projectname
      axios.get(sentiment_path)
        .then(({ data }) => {
          this.lineChartData = data.sentiment
        })
        .catch(error => {
          console.log(error)
        })
      axios.get(information_path)
        .then(({ data }) => {
          this.pieChartData = data.pieChartData
          this.issuenum = data.issuenum
        })
        .catch(error => {
          console.log(error)
        })
      axios.get(contributor_path)
        .then(({ data }) => {
          this.contributors = data.contributors
          this.contributorsnum = data.contributorsnum
          this.recommend = data.recommend
        })
        .catch(error => {
          console.log(error)
        })
    },
    handleSearch() {
      this.searchLoading = true
      this.fetchData(this.ownername, this.projectname)
      this.searchLoading = false
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
