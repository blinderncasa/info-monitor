<template>
  <div class="hello">
    <h2>{{ `${now.getHours()}:${now.getMinutes()}:${now.getSeconds()}` }}</h2>
    <div>
      <p>Mot sentrum:</p>
      <p>{{ getDeparturesFromPlatform(departures, "1").length > 0 ? getDeparturesFromPlatform(departures, "1")[0].lineCode + ' - ' + getDeparturesFromPlatform(departures, "1")[0].destination + ' (' + getHumanTime(getDeparturesFromPlatform(departures, "1")[0].depature) + ' min)': '' }}</p>
      <p>{{ getDeparturesFromPlatform(departures, "1").length > 1 ? getDeparturesFromPlatform(departures, "1")[1].lineCode + ' - ' + getDeparturesFromPlatform(departures, "1")[1].destination + ' (' + getHumanTime(getDeparturesFromPlatform(departures, "1")[1].depature) + ' min)': '' }}</p>
    </div>
    <div>
      <p>Mot ringen:</p>
      <p>{{ getDeparturesFromPlatform(departures, "2").length > 0 ? getDeparturesFromPlatform(departures, "2")[0].lineCode + ' - ' + getDeparturesFromPlatform(departures, "2")[0].destination + ' (' + getHumanTime(getDeparturesFromPlatform(departures, "2")[0].depature) + ' min)': '' }}</p>
      <p>{{ getDeparturesFromPlatform(departures, "2").length > 1 ? getDeparturesFromPlatform(departures, "2")[1].lineCode + ' - ' + getDeparturesFromPlatform(departures, "2")[1].destination + ' (' + getHumanTime(getDeparturesFromPlatform(departures, "2")[1].depature) + ' min)': '' }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      departures: [],
      now: new Date()
    }
  },
  mounted () {
    this.loadData();

    setInterval(function () {
      this.loadData();
    }.bind(this), 30000);

    setInterval(() => {
      this.now = new Date()
    }, 1000);
  },
  methods: {
    loadData() {
      this.$http.get('https://europe-west1-focused-brand-202320.cloudfunctions.net/depatures/NSR:StopPlace:6332').then(response => {
        this.departures = response.body;
      }, error => {
        console.log(error);
      });
    },
    getDeparturesFromPlatform: (departures, platform) => {
      return departures.filter((departure) => {
        return departure.platform === platform;
      })
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
