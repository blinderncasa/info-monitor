<template>
    <div class="white-box">

        <div v-for="bikeStation in bikeStations" :key="bikeStation.index">
            <div class="single"><div class="text">{{stationToNameDesc[bikeStation.station_id].name }}<p>{{ stationToNameDesc[bikeStation.station_id].desc }}</p></div>  <span><img src="../assets/bicycle.svg" alt="" />{{ bikeStation.num_bikes_available}} </span></div>
        </div>

    </div>
</template>

<script>

    export default {
        name: "CityBike",
        data (){
            return {
                stationIds: [ "474", "496", "580", "619", "486"],
                stationToNameDesc: {
                    "474": {
                        name: 'Blindern studentparkering',
                        desc: 'Blindern studentparkering',
                    },
                    "496": {
                        name: 'Fysikkbygningen',
                        desc: 'Blindern Fysikkbygg'
                    },
                    "580": {
                        name: 'Georg Morgenstiernes hus',
                        desc: 'Blindern  Moltke Moes vei'
                    },
                    "619": {
                        name: 'Bak Niels Treschows hus nord',
                        desc: 'Bak Niels Treschows nord'
                    },
                    "486": {
                        name: 'Det Teologiske fakultet',
                        desc: 'Universitetet i Oslo'
                    },
                },
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
                this.$http.get('https://gbfs.urbansharing.com/oslobysykkel.no/station_status.json').then(response => {
                    this.bikeStations = [];
                    let tmpStations = [];
                    let tmpStations2 = [];

                    for (let i = 0; i < this.stationIds.length ; i++) {
                        tmpStations[i] = response.body.data.stations.find((item) => {
                            return item.station_id === this.stationIds[i];
                        });
                    }

                    tmpStations.forEach((item) => {
                        if(item.num_bikes_available > 0) {
                            tmpStations2.push(item);
                        }
                    });

                    this.bikeStations = tmpStations2.concat(tmpStations).slice(0,2);

                }, error => {
                    return error;
                });
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