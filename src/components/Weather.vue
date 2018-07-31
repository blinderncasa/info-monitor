<template>
    <div class="weather">
        <div class="single-weather" v-for="(forcast, index) in weather" v-bind:key="index"> <!-- forcast.location.symbol.id -->
            <img class="icon" src="../assets/weather/regn.svg" alt="regn" />
            <span class="temp">
                {{ parseInt(((parseInt(forcast.location.minTemperature.value) + parseInt(forcast.location.maxTemperature.value)) / 2) + 0.5) }}
            </span>
            <span class="time">
                {{ getHumanTime(new Date(forcast.from)) + ' - ' + getHumanTime(new Date(forcast.to)) }}
            </span>
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
        if(time.getHours() < 10) {
            return '0' + time.getHours();
        }
        return time.getHours();
      },
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .weather{
        color: white;
        display: flex;
    }
    .weather .icon{
        width: 84px;
        padding-bottom: 10px;
    }
    .single-weather{
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 10px;
    }
    .single-weather .temp{
        font-size: 44px;
    }
    .single-weather .temp:after{
        content: '°';
        margin-left: -7px;
    }
    .single-weather .time{
        color: rgba(255,255,255,0.36);
        font-weight: 300;
    }
</style>
