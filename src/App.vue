<template>
  <div id="app">
    <Clock/>
    <PublicTransit/>
    <CityBike/>
  </div>
</template>

<script>
import PublicTransit from './components/PublicTransit.vue'
import Clock from './components/Clock.vue'
import CityBike from "./components/CityBike";

export default {
  name: 'app',
  components: {
      CityBike,
    Clock,
    PublicTransit
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
