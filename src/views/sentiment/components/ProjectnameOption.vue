<template>
  <div style="display:inline-block;">
    <!-- $t is vue-i18n global function to translate lang -->
    <label class="radio-label" style="padding-left:0;">Project Name:</label>
    <el-input :placeholder="$t('sentiment.placeholder')" v-model="projectname" style="width:250px;" prefix-icon="el-icon-star-off"/>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: ''
    }
  },
  computed: {
    projectname: {
      get() {
        return this.value
      },
      set(val) {
        this.$emit('input', val)
      }
    }
  }
}
</script>
