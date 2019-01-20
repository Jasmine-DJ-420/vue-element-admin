<template>
  <div class="createPost-container">
    <div>
      <el-button v-loading="loading" style="margin-left: 50px;" type="success" round="" @click="searchSentiment">{{ $t('sentiment.analysis') }}</el-button>
      <el-tag type="warning" style="margin-left: 20px;">Sentiment: {{ sentiment }}</el-tag>
      <el-tag type="danger" style="margin-left: 20px">Entity: {{ entity }}</el-tag>
    </div>
    <div class="createPost-main-container">
      <el-row>
        <el-col :span="24">
          <div class="editor-container">
            <Tinymce ref="editor" :height="300" v-model="textContent" toolbar="[]" menubar="file edit view format"/>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Tinymce from '@/components/Tinymce'
import Upload from '@/components/Upload/singleImage3'
import MDinput from '@/components/MDinput'

export default {
  name: 'SentimentAnalysis',
  components: { Tinymce, MDinput, Upload },
  props: {
  },
  data() {
    return {
      loading: false,
      textContent: '',
      sentiment: 'None',
      entity: 'None'
    }
  },
  computed: {
    lang() {
      return this.$store.getters.language
    }
  },
  created() {
  },
  methods: {
    searchSentiment() {
      this.loading = true
      var contentText = this.textContent.substring(3, this.textContent.length - 4)
      const path = `http://127.0.0.1:2345/api/sentiment_analysis`
      axios.post(path, { content: contentText })
        .then(({ data }) => {
          this.sentiment = data.sentiment
          this.entity = data.entity
        })
        .catch(error => {
          console.log(error)
        })
      this.loading = false
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  @import "src/styles/mixin.scss";
  .createPost-container {
    position: relative;
    .createPost-main-container {
      padding: 40px 45px 20px 50px;
      .postInfo-container {
        position: relative;
        @include clearfix;
        margin-bottom: 10px;
        .postInfo-container-item {
          float: left;
        }
      }
      .editor-container {
        min-height: 400px;
        margin: 0 0 30px;
        .editor-upload-btn-container {
          text-align: right;
          margin-right: 10px;
          .editor-upload-btn {
            display: inline-block;
          }
        }
      }
    }
    .word-counter {
      width: 40px;
      position: absolute;
      right: -10px;
      top: 0px;
    }
  }
</style>
