<template>
  <div class="clock">
    <h1>{{ `${now.toTimeString().substring(0, now.toTimeString().indexOf(' ')-3)}` }}</h1>
  </div>
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
  h1{
    margin-top: 0;
    color: white;
    font-size: 64px;
  }
  .clock{

  }
</style>
