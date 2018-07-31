<template>
    <div class="weather">
        <div class="single-weather" v-for="(forcast, index) in weather" v-bind:key="index"> <!-- forcast.location.symbol.id -->
            <img class="icon" :src="require('@/assets/weather/' + weatherToIconMapper[forcast.location.symbol.number])" />
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
        weatherToIconMapper: {
          1: 'sol.svg',
          2: 'sky-sol.svg',
          3: 'sky-sol.svg',
          15: 'sky.svg',
          6: 'regn-lyn.svg',
          24: 'regn-lyn.svg',
          25: 'regn-lyn.svg',
          26: 'regn-lyn.svg',
          20: 'regn-lyn.svg',
          27: 'regn-lyn.svg',
          28: 'regn-lyn.svg',
          21: 'regn-lyn.svg',
          29: 'regn-lyn.svg',
          30: 'regn-lyn.svg',
          22: 'regn-lyn.svg',
          11: 'regn-lyn.svg',
          31: 'regn-lyn.svg',
          23: 'regn-lyn.svg',
          32: 'regn-lyn.svg',
          33: 'regn-lyn.svg',
          14: 'regn-lyn.svg',
          34: 'regn-lyn.svg',
          40: 'regn.svg',
          5: 'regn.svg',
          41: 'regn.svg',
          42: 'regn.svg',
          7: 'regn.svg',
          43: 'regn.svg',
          44: 'regn.svg',
          8: 'regn.svg',
          45: 'regn.svg',
          46: 'regn.svg',
          9: 'regn.svg',
          10: 'regn.svg',
          47: 'regn.svg',
          12: 'regn.svg',
          48: 'regn.svg',
          49: 'regn.svg',
          13: 'regn.svg',
          50: 'regn.svg',
        }
      }
    },
    mounted () {
      this.loadData();

      setInterval(function () {
        this.loadData();
      }.bind(this), 15 * 60000);
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
        height: 60px;
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
