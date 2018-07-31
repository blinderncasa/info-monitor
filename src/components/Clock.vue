<template>
  <div class="clock">
    <h1>{{ `${now.toTimeString().substring(0, now.toTimeString().indexOf(' ')-3)}` }}</h1>
    <p>{{ now.getDate() + '. ' + monthNumToStringMapper[now.getMonth()] + ' ' + now.getFullYear() }}</p>
  </div>
</template>

<script>
export default {
  name: 'Clock',
  data () {
    return {
      now: new Date(),
      monthNumToStringMapper: {
          0: 'januar',
          1: 'februar',
          2: 'mars',
          3: 'april',
          4: 'mai',
          5: 'juni',
          6: 'juli',
          7: 'august',
          8: 'september',
          9: 'oktober',
          10: 'november',
          11: 'desember',
      }
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
  h1{
    margin-top: 0;
    color: white;
    font-size: 102px;
    margin-bottom: 0;
  }
  .clock{

  }
  p{
    color: white;
    margin-top: 0;
    font-size: 32px;
  }
</style>
