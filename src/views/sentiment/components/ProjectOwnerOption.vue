<template>
  <div style="display:inline-block;margin-right: 20px">
    <label class="radio-label" style="padding-left:0;">Project Owner:&nbsp;&nbsp;&nbsp;</label>
    <el-input :placeholder="$t('sentiment.placeholderowner')" v-model="ownername" style="width:335px;" prefix-icon="el-icon-star-off"/>
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
    ownername: {
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
