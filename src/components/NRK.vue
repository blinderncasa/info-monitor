<template>
    <div class="white-box">
        <div v-if="data[0]" class="main">
            <h3>{{ data[0].title }}</h3>
            <div class="date">{{ getHumanTime(new Date(data[0].pubDate)) }}</div>
            <p>{{ data[0].content }}</p>
        </div>
        <div class="list">
            <div v-for="news in data.slice(1, 4)">
                <h3>{{ news.title }}</h3>
                <div class="date">{{ getHumanTime(new Date(news.pubDate)) }}</div>
            </div>
        </div>
    </div>
</template>

<script>
  import Parser from 'rss-parser'

  export default {
    name: "NRK",
    data() {
      return {
        data: [],
        parser: new Parser(),
      };
    },
    mounted() {
      this.getData();

      setInterval(() => {
        this.now = new Date()
      }, 30000);
    },
    methods: {
      getData() {
        this.parser.parseURL('https://thingproxy.freeboard.io/fetch/https://www.nrk.no/nyheter/siste.rss', (err, feed) => {
          this.data = feed.items;
        });
      },
        getHumanTime: (time) => {
        let hours = time.getHours();
  if(hours < 10) {
      hours = '0' + hours;
  }
  let minutes = time.getMinutes();
  if(minutes < 10) {
      minutes = '0' + minutes;
  }
  return hours + ':' + minutes;
  },
    }
  }
</script>

<style scoped>
    .white-box {
        display: flex;
        justify-content: space-between;
    }
    .main{
        width: 50%;
    }
    h3{
        font-size: 23px;
        margin-bottom: 8px;
    }
    .date{
        color: gray;
    }
    p{
        font-size: 18px;
        margin-top: 8px;
    }
</style>