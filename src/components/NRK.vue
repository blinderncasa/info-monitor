<template>
    <div class="white-box">
        <div v-if="data[0]">
            <div>{{ data[0].title }}</div>
            <div>{{ data[0].pubDate }}</div>
            <div>{{ data[0].content }}</div>
        </div>
        <div>
            <div v-for="news in data.slice(1, 4)">
                <div>{{ news.title }}</div>
                <div>{{ news.pubDate }}</div>
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
      }, 10000);
    },
    methods: {
      getData() {
        this.parser.parseURL('https://jsonp.afeld.me/?url=https%3A%2F%2Fwww.nrk.no%2Fnyheter%2Fsiste.rss', (err, feed) => {
          this.data = feed.items;
        });
      }
    }
  }
</script>

<style scoped>
.white-box {
    display: flex;
    justify-content: center;
}
</style>