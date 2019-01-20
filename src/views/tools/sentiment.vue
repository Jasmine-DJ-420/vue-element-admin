<template>
  <div class="app-container">
    <div class="dashboard-editor-container">
      <sentiment-analysis/>
    </div>
  </div>

</template>

<script>
import SentimentAnalysis from './components/SentimentAnalysis'

export default {
  name: 'Sentiment',
  components: { SentimentAnalysis },
  data() {
    return {
    }
  },
  created() {
  },
  methods: {
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
