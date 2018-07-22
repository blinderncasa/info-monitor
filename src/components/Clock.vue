<template>
  <h1>{{ `${now.getHours()}:${now.getMinutes()}:${now.getSeconds()}` }}</h1>
</template>

<script>
export default {
  name: 'Clock',
  data () {
    return {
      now: new Date()
    }
  },
  mounted () {
    setInterval(() => {
      this.now = new Date()
    }, 1000);
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
