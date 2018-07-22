<template>
    <div>
        <h2>Bysykkelstativer</h2>
        <div v-for="bikeStation in bikeStations">
            <p>{{bikeStation.name + ': ' + bikeStation.bikesAvailable}}</p>
        </div>
    </div>
</template>

<script>
    export default {
        name: "CityBike",
        data (){
            return {
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
                this.$http.post('https://api.entur.org/journeyplanner/2.0/index/graphql',
                '{"query":"\\nquery bikeRentalStationsByBox($minLat:Float, $minLng:Float, $maxLat:Float, $maxLng:Float) {\\n  bikeRentalStationsByBbox(minimumLatitude: $minLat, minimumLongitude: $minLng, maximumLatitude: $maxLat , maximumLongitude: $maxLng) {\\n    id\\n    name\\n    bikesAvailable\\n    spacesAvailable\\n    longitude\\n    latitude\\n  }\\n}","variables":{"minLng":10.709744182933237,"minLat":59.93510039818138,"maxLng":10.727697817066762,"maxLat":59.94409360181862}}'
                ).then(response => {
                    this.bikeStations = response.data.data.bikeRentalStationsByBbox;

                }, error => {
                    return error;
                });
            }
        }

    }
</script>

<style scoped>

</style>