<template>
  <div class="white-box">
    <div class="one-way">
      <div class="tekst">
        <p>Mot sentrum</p>
        <span>Vestli, Bergkrystallen, Ringen</span>
      </div>
      <div class="departures">
        <div v-for="depature in getDeparturesFromPlatform(departures, '1')" class="single" v-bind:class="{ blue: depature.lineCode === 4  || depature.destination === 'Ringen via Majorstuen' }">{{ getHumanTime(depature.depature) + ' min' }}</div>
      </div>
    </div>
    <div class="one-way">
      <div class="tekst">
        <p>Mot ringen</p>
        <span>Sognsvann, Ringen, Vestli</span>
      </div>
      <div class="departures">
        <div v-for="depature in getDeparturesFromPlatform(departures, '2')" class="single" v-bind:class="{ blue: depature.lineCode === 4 }">{{ getHumanTime(depature.depature) + ' min' }}</div>
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
    getDeparturesFromPlatform(departures, platform) {
      return departures.filter((departure) => {
        return departure.platform === platform && this.getHumanTime(departure.depature) !== 0;
      }).slice(0, 2)
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

  .one-way .blue {
    color: blue !important;
    border-color: blue !important;
  }

  .one-way span{
    text-transform: uppercase;
    color: #BABABA;
  }

</style>
