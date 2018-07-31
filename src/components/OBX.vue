<template>
  <div v-if="data">
    <div>{{ data.CHANGE_PCT.toFixed(2) }}</div>
    <div>{{ data.CHANGE_7DAYS_PCT.toFixed(2) }}</div>
    <div>{{ data.CHANGE_1MONTH_PCT.toFixed(2) }}</div>
  </div>
</template>

<script>
export default {
  name: 'OBX',
  data () {
    return {
      data: null,
    }
  },
  mounted () {
    console.log("kjør");
    this.loadData();

    setInterval(function () {
      this.loadData();
    }.bind(this), 30000);
  },
  methods: {
    loadData() {
      this.$http.get('https://jsonp.afeld.me/?url=https://www.dn.no/finans/servlets/newt/json/quotes?ticker=OBX.OSE').then(response => {
        this.data = response.body[0];
      }, error => {
        return error;
      });
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
