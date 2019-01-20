<template>
  <div style="display:inline-block;">
    <label class="radio-label" style="padding-left:0;">Developer name:</label>
    <el-input :placeholder="$t('sentiment.placeholderdeveloper')" v-model="developername" style="width:335px;" prefix-icon="el-icon-star-off"/>
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
    developername: {
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
