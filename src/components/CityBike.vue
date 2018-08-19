<template>
    <div class="white-box">
        <h2>Bysykkelstativer</h2>
        <div v-for="bikeStation in bikeStations" v-bind:key="bikeStation.id">
            <p>{{bikeStation.name +  ' - ' + bikeStation.description + ' : ' + bikeStation.bikesAvailable}}</p>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue'

    export default {
        name: "CityBike",
        data (){
            return {
                stationIds: [ "308", "304", "305", "184", "309"],
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
                        return Vue.http.post('https://api.entur.org/journeyplanner/2.0/index/graphql', '{"query":"{\\n  bikeRentalStation(id: \\"' + stationId + '\\") {\\n    name\\n    id\\n    realtimeOccupancyAvailable\\n    bikesAvailable\\n description\\n    spacesAvailable\\n  }\\n}\\n","variables":null,"operationName":null}\n')
                    }))
                }

                function sortBikes(bikeStations){
                    let tmpStations = [];
                    let sortedStations = [];

                    for (let i = 0; i < bikeStations.length ; i++) {
                        tmpStations[i] = bikeStations[i].body.data.bikeRentalStation;
                    }

                    sortedStations = tmpStations.sort(((a, b) => a.bikesAvailable < b.bikesAvailable ? 1 : -1));

                    if(tmpStations[0].bikesAvailable === 0 && tmpStations[1].bikesAvailable === 0 ){
                        this.bikeStations = sortedStations;
                        return;
                    }

                    if(tmpStations[0].bikesAvailable === 0){
                        var tmp = tmpStations[0];
                        tmpStations[0] = sortedStations[0];
                    }
                    if(tmpStations[1].bikesAvailable === 0){
                        var tmp = tmpStations[1];
                        tmpStations[1] = sortedStations[0];
                    }

                    this.bikeStations = tmpStations;



                }

                bikeRequest(this.stationIds).then(sortBikes.bind(this));

            }
        }

    }
</script>

<style scoped>

</style>