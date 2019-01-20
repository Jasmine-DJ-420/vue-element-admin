<template>
  <div class="app-container">
    <div class="dashboard-editor-container">
      <panel-group :cur-ownername="curOwnername" :cur-projectname="curProjectname"/>
    </div>
  </div>

</template>

<script>
import PanelGroup from './components/PanelGroup'

export default {
  name: 'Index',
  components: { PanelGroup },
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
