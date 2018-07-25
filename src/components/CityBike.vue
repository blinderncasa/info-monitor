<template>
    <div class="white-box">
        <h2>Bysykkelstativer</h2>
        <div v-for="bikeStation in bikeStations" v-bind:key="bikeStation.id">
            <p>{{bikeStation.name + ': ' + bikeStation.bikesAvailable}}</p>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue'

    export default {
        name: "CityBike",
        data (){
            return {
                stationIds: ["184", "304", "305", "308", "309"],
                bikeStations: [],
            }

        },

        mounted (){
            this.loadData();


            setInterval(function () {

                this.loadData();
            }.bind(this), 30000);
        },

        methods: {
            loadData(){
                function bikeRequest(stationIds){
                    return Promise.all(stationIds.map(function (stationId) {
                        return Vue.http.post('https://api.entur.org/journeyplanner/2.0/index/graphql', '{"query":"{\\n  bikeRentalStation(id: \\"' + stationId + '\\") {\\n    name\\n    id\\n    realtimeOccupancyAvailable\\n    bikesAvailable\\n    spacesAvailable\\n  }\\n}\\n","variables":null,"operationName":null}\n')
                    }))
                }

                function sortBikes(bikeStations){
                    let tmpStations = [];
                    for (let i = 0; i < bikeStations.length ; i++) {
                        tmpStations[i] = bikeStations[i].body.data.bikeRentalStation;
                    }
                    this.bikeStations = tmpStations.sort(((a, b) => a.bikesAvailable < b.bikesAvailable ? 1 : -1))
                }

                bikeRequest(this.stationIds).then(sortBikes.bind(this));

            }
        }

    }
</script>

<style scoped>

</style>