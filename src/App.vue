<template>
  <div id="app">
    <Header />
    <Quotes
        :price="price"
        :getPrice="getPrise"
        :runWebSocket="runWebSocket"
        :currencyPairs="currencyPairs"
    />
    <h1>Hello World</h1>
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
    price: [],

    currencyPairs: [
      {
        pair: ['USD', 'BTC'],
        price: {},
      },
      {
        pair: ['USD', 'BCH'],
        price: {},
      },
      {
        pair: ['USD', 'ETH'],
        price: {},
      },
      {
        pair: ['USD', 'XRP'],
        price: {},
      },
    ],
    selectedPair: '',
  }),

  methods: {
    getPrise (currency, crypto, limit) {
      fetch(`https://min-api.cryptocompare.com/data/v2/histominute?fsym=${crypto}&tsym=${currency}&limit=${limit}`)
        .then(response => response.json())
    },

    runWebSocket() {
      const getPairs = this.currencyPairs.map(currencyPair => {
        return `0~Coinbase~${currencyPair.pair[1]}~${currencyPair.pair[0]}`
      })
      const apiKey = "ba496cd06cf8fca4052b30ae30d4067e0e8988c429ef71e5fc887aeaff9de89e";
      let ccStreamer = new WebSocket('wss://streamer.cryptocompare.com/v2?api_key=' + apiKey);
      ccStreamer.onopen = function onStreamOpen() {
        let subRequest = {
          "action": "SubAdd",
          "subs": getPairs,
        };
        ccStreamer.send(JSON.stringify(subRequest));
      }

      ccStreamer.onmessage = (event) => {
        const message = JSON.parse(event.data);
        const price = message.P;
        if(price) {
          const pair = this.currencyPairs.find(currencyPair =>
              currencyPair.pair.join('') === message.TSYM + message.FSYM
          )
          pair.price = price.toFixed(2)
        }
      }
    },

      setSelectedPair(pair) {
        this.selectedPair = pair;
      },
    },


  created() {
    this.setSelectedPair(this.currencyPairs[0].pair)
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
