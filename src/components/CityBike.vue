<template>
    <div class="white-box">

        <div v-for="bikeStation in bikeStations" :key="bikeStation.index">
            <div class="single"><div class="text">{{bikeStation.name }}<p>{{ bikeStation.description }}</p></div>  <span><img src="../assets/bicycle.svg" alt="" />{{ bikeStation.bikesAvailable}} </span></div>
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
                    this.bikeStations = [];
                    let tmpStations = [];
                    let sortedStations = [];

                    for (let i = 0; i < bikeStations.length ; i++) {
                        tmpStations[i] = bikeStations[i].body.data.bikeRentalStation;

                    }

                    sortedStations = tmpStations;

                    sortedStations = sortedStations.sort(((a, b) => a.bikesAvailable < b.bikesAvailable ? 1 : -1));

                    if(tmpStations[0].bikesAvailable === 0 && tmpStations[1].bikesAvailable === 0 ){
                        this.bikeStations = sortedStations.slice(0,2);
                        return;
                    }

                    if(tmpStations[0].bikesAvailable === 0){

                        tmpStations[0] = sortedStations[0];
                    }
                    if(tmpStations[1].bikesAvailable === 0){

                        if(tmpStations[0] === sortedStations[0]){
                            tmpStations[1] = sortedStations[1];
                        }
                        else{
                            tmpStations[1] = sortedStations[0];
                        }

                    }

                    this.bikeStations = tmpStations.slice(0,2);




                }
                bikeRequest(this.stationIds).then(sortBikes.bind(this));
            }
        }
    }
</script>

<style scoped>
    .single{
        font-size: 30px;
        padding: 15px 0 15px 0;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .single span{
        border: 2px solid #015EC5;
        width: 100px;
        padding: 15px 40px;
        border-radius: 8px;
        text-align: center;
        color: #015EC5;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .single p{
        font-size: 16px;
        text-transform: uppercase;
        color: #A3A3A3;
    }
    .single span img{
        width: 50px;
        margin-right: 10px;
    }
</style>