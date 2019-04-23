<template>
  <div id="app">
    <div class="clock-weather-wrapper">
      <Clock/>
      <Weather/>
    </div>
    <PublicTransit/>
    <CityBike/>
    <NRK/>
    <OBX/>
  </div>
</template>

<script>
import PublicTransit from './components/PublicTransit.vue'
import Clock from './components/Clock.vue'
import CityBike from "./components/CityBike";
import Weather from "./components/Weather";
import NRK from "./components/NRK";
import OBX from "./components/OBX";

export default {
  name: 'app',
  components: {
    CityBike,
    Clock,
    PublicTransit,
    Weather,
    NRK,
    OBX,
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Muli');

  body{
    padding: 0;
    margin: 0;
    font-family: 'Muli', sans-serif;
  }
  #app {
    background-image: url("assets/bg2.jpg");
    background-size: cover;
    height: 100vh;
    background-position: bottom;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .clock-weather-wrapper{
    width: 80vw;
    display: flex;
    justify-content: space-between;
  }
  .white-box{
    width: 80vw;
    background-color: white;
    margin-bottom: 50px;
    padding: 50px;
  }
</style>
