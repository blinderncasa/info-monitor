<template>
  <div class="white-box">
    <div class="one-way">
      <div class="tekst">
        <p>Mot sentrum</p>
        <span>Vestli, Bergkrystallen, Ringen</span>
      </div>
      <div class="departures">
        <div class="single">{{ getDeparturesFromPlatform(departures, "1").length > 0 ? getHumanTime(getDeparturesFromPlatform(departures, "1")[0].depature) + ' min': '' }}</div>
        <div class="single">{{ getDeparturesFromPlatform(departures, "1").length > 1 ? getHumanTime(getDeparturesFromPlatform(departures, "1")[1].depature) + ' min': '' }}</div>
      </div>
    </div>
    <div class="one-way">
      <div class="tekst">
        <p>Mot ringen</p>
        <span>Sognsvann, Ringen, Vestli</span>
      </div>
      <div class="departures">
        <div class="single">{{ getDeparturesFromPlatform(departures, "2").length > 0 ? getHumanTime(getDeparturesFromPlatform(departures, "2")[0].depature) + ' min': '' }}</div>
        <div class="single">{{ getDeparturesFromPlatform(departures, "2").length > 1 ? getHumanTime(getDeparturesFromPlatform(departures, "2")[1].depature) + ' min': '' }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PublicTransit',
  data () {
    return {
      departures: [],
    }
  },
  mounted () {
    this.loadData();

    setInterval(function () {
      this.loadData();
    }.bind(this), 30000);
  },
  methods: {
    loadData() {
      this.$http.get('https://europe-west1-focused-brand-202320.cloudfunctions.net/depatures/NSR:StopPlace:6332').then(response => {
        this.departures = response.body;
      }, error => {
        return error;
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

  .one-way{
    display: flex;
    justify-content: center;
    justify-content: space-between;
    align-items: center;
    padding: 15px 0 15px 0;
  }
  .one-way p{
    font-size: 30px;
    margin-bottom: 0;
    margin-top: 0;
  }
  .departures{
    display: flex;
  }
  .one-way .single{
    font-size: 30px;
    border: 2px solid green;
    padding: 15px 40px;
    margin-left: 40px;
    border-radius: 8px;
    color: green;
    width: 100px;
    text-align: center;
  }
  .one-way span{
    text-transform: uppercase;
    color: #BABABA;
  }

</style>
