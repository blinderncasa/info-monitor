<template>
    <div>
        <h2>Været</h2>
        <div v-for="(forcast, index) in weather" v-bind:key="index">
            {{ (new Date(forcast.from)).getHours() + ':00 - ' + (new Date(forcast.to)).getHours() + ':00 ' + forcast.location.minTemperature.value + ' - ' + forcast.location.maxTemperature.value }}
        </div>
    </div>
</template>

<script>
  export default {
    name: 'Weather',
    data () {
      return {
        weather: [],
      }
    },
    mounted () {
      this.loadData();

      setInterval(function () {
        this.loadData();
      }.bind(this), 15*60000);
    },
    methods: {
      loadData() {
        this.$http.get('https://us-central1-lilleengen-io.cloudfunctions.net/blindern-yr').then(response => {
          this.weather = response.body.weatherdata.product.time.filter((forcast) => {
            const time = new Date(forcast.from);
            return !!forcast.location.minTemperature && [0, 6, 12, 18].includes(time.getHours())
          }).slice(0, 4);
        }, error => {
          return error;
        });
      },
      getHumanTime: (time) => {
        return Math.round((Date.parse(time) - Date.now())/(60*1000))
      },
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
