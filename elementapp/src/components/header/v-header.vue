<template>
  <div class="header">
  header头部
  </div>
</template>

<script>
export default {
  name: '',
  data () {
    return {
      msg: ''
    }
  },
  created () {
  	this.$http({
      url: `https://cnodejs.org/api/v1/${this.$route.path}`,
      method: 'get'
    }).then(res => {
      this.titles = res.data.data
    }).catch(error => {
      console.log(error)
    })
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
</style>
