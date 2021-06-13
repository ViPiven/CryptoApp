<template>
  <div id="app">
    <Header />
    <Quotes :price="price"/>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Quotes from "./components/Quotes";

export default {
  name: 'App',
  components: {
    Quotes,
    Header
  },

  data: () => ({
    price: 0,
  }),

  methods: {
    runWebSocket() {
      // this is where you paste your api key
      let apiKey = "ba496cd06cf8fca4052b30ae30d4067e0e8988c429ef71e5fc887aeaff9de89e";
      let ccStreamer = new WebSocket('wss://streamer.cryptocompare.com/v2?api_key=' + apiKey);
      ccStreamer.onopen = function onStreamOpen() {
        let subRequest = {
          "action": "SubAdd",
          "subs": ["0~Coinbase~BTC~USD"]
        };
        ccStreamer.send(JSON.stringify(subRequest));
      }

      ccStreamer.onmessage = (event) => {
        const message = JSON.parse(event.data);
        this.price = message.P;
        console.log("Received from Cryptocompare: " + message.P);
      }
    }
  },

  created() {
    this.runWebSocket()
  }
}

</script>

<style lang="scss">
#app {
  box-sizing: border-box;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #000;
}
body {
  margin: 0;
  padding: 0;
  overflow: hidden;
}

h1, h2, h3, h4, h5 {
  margin: 0;
}

</style>
