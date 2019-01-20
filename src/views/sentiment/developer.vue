<template>
  <div class="app-container">
    <div class="dashboard-editor-container">
      <el-row :gutter="32">
        <el-col :xs="24" :sm="24" :lg="8">
          <div class="chart-wrapper">
            <developer-information :information-data="developerInformation"/>
          </div>
        </el-col>
        <el-col :xs="24" :sm="24" :lg="8">
          <div class="chart-wrapper">
            <span class="table-name">Follow Bar Chart</span>
            <follow-information :chart-data="followInformation"/>
          </div>
        </el-col>
        <el-col :xs="24" :sm="24" :lg="8">
          <div class="chart-wrapper">
            <span class="table-name">Sentiment Expression Bar Chart</span>
            <express-sentiment-information :chart-data="expressInformation"/>
          </div>
        </el-col>
      </el-row>

      <panel-group :panel-data="panelData"/>

      <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
        <span class="table-name">Sentiment Timeline</span>
        <line-chart :chart-data="lineChartData"/>
      </el-row>

      <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
        <span class="table-name">Sentiment Relation</span>
        <force-chart :chart-data="relation"/>
      </el-row>
    </div>
  </div>

</template>

<script>
import axios from 'axios'

// options components
import DeveloperInformation from './developer-components/DeveloperInformation'
import FollowInformation from './developer-components/FollowInformation'
import ExpressSentimentInformation from './developer-components/ExpressSentimentInformation'
import PanelGroup from './developer-components/PanelGroup'
import LineChart from './components/LineChart'
import ForceChart from './developer-components/ForceChart'

export default {
  name: 'Developer',
  components: { DeveloperInformation, FollowInformation, ExpressSentimentInformation, PanelGroup, LineChart, ForceChart },
  data() {
    return {
      developerInformation: [],
      followInformation: {
        followers: 0,
        following: 0
      },
      expressInformation: {
        expressing: {
          positive: 0,
          negative: 0
        },
        expressed: {
          positive: 0,
          negative: 0
        }
      },
      lineChartData: {
        axis: [],
        positive: [],
        negative: []
      },
      relation: {
        chartNode: [],
        chartLink: []
      },
      ownerName: '',
      projectName: '',
      developerName: '',
      panelData: {
        issue: 0,
        comment: 0,
        positive: 0,
        negative: 0
      }
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.developerName = this.$route.params && this.$route.params.login
      this.ownerName = this.$route.params && this.$route.params.owner
      this.projectName = this.$route.params && this.$route.params.project
      const information_path = `http://127.0.0.1:2345/api/` + this.developerName + `/information`
      const relation_path = `http://127.0.0.1:2345/api/` + this.ownerName + `/` + this.projectName + `/` + this.developerName + `/relation`
      const personality_path = `http://127.0.0.1:2345/api/` + this.ownerName + `/` + this.projectName + `/` + this.developerName + `/personality`
      const statistics_path = `http://127.0.0.1:2345/api/` + this.ownerName + `/` + this.projectName + `/` + this.developerName + `/statistics`
      const sentiment_path = `http://127.0.0.1:2345/api/` + this.ownerName + `/` + this.projectName + `/` + this.developerName + `/emotion_timing`
      axios.get(information_path)
        .then(({ data }) => {
          this.developerInformation = data.developerInformation
          this.followInformation = data.followInformation
        })
        .catch(error => {
          console.log(error)
        })
      axios.get(personality_path)
        .then(({ data }) => {
          this.expressInformation = data.expressInformation
        })
        .catch(error => {
          console.log(error)
        })
      axios.get(statistics_path)
        .then(({ data }) => {
          this.panelData = data.panelData
        })
        .catch(error => {
          console.log(error)
        })
      axios.get(sentiment_path)
        .then(({ data }) => {
          this.lineChartData = data.sentiment
        })
        .catch(error => {
          console.log(error)
        })
      axios.get(relation_path)
        .then(({ data }) => {
          this.relation = data.relation
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
