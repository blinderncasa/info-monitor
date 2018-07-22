<template>
  <h1>{{ `${now.toTimeString().substring(0, now.toTimeString().indexOf(' '))}` }}</h1>
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

  methods: {
      getDate() {

      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
