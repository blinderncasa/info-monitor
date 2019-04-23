<template>
  <div class="obx" v-if="data">
    <div>
      <div class="single-info">
        <span class="desc">Siste dag</span>
        <span class="percent">{{ data.CHANGE_PCT.toFixed(2) }}</span>
      </div>
      <div class="single-info">
        <span class="desc">Siste uke</span>
        <span class="percent">{{ data.CHANGE_7DAYS_PCT.toFixed(2) }}</span>
      </div>
      <div class="single-info">
        <span class="desc">Siste måned</span>
        <span class="percent">{{ data.CHANGE_1MONTH_PCT.toFixed(2) }}</span>
      </div>
    </div>
  <span class="absolute-percent">%</span>
  <span class="obx-title">OBX</span>
</div>
</template>

<script>
export default {
  name: 'OBX',
  data () {
    return {
      data: null,
    }
  },
  mounted () {
    console.log("kjør");
    this.loadData();

    setInterval(function () {
      this.loadData();
    }.bind(this), 30000);
  },
  methods: {
    loadData() {
      this.$http.get('https://jsonp.afeld.me/?url=https://www.dn.no/finans/servlets/newt/json/quotes?ticker=OBX.OSE').then(response => {
        this.data = response.body[0];
      }, error => {
        return error;
      });
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .obx{
    display: flex;
    align-items: center;
  }
  .single-info{
    color: white;
    height: 50px;
  }
  .desc{
    text-transform: uppercase;
    font-size: 18px;
    color: #fff;
    font-weight: 300;
    min-width: 150px;
    display: inline-block;
    text-align: right;
  }
  .percent{
    font-size: 28px;
    color: white;
    margin-left: 70px;
    vertical-align: middle;
  }
  .absolute-percent{
    position: relative;
    font-size: 140px;
    color: white;
    margin-left: 40px;
  }
  .obx-title{
    font-size: 58px;
    color: white;
    margin-left: 40px;
  }
</style>
