<template lang="html">
  <div>
<div class="container">
  <h3>Crypto-Watch</h3>


  <div>
    <table class="table table-hover">
      <thead>
        <tr>
          <td>Rank</td>
          <td>Name</td>
          <td>Symbol</td>
          <td>Price (GBP)</td>
          <td>1H</td>
          <td>1D</td>
          <td>1W</td>
          <td>Market Cap (GBP)</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="coin in cryptoData">
          <td>{{coin.market_cap_rank}}</td>
          <td>{{coin.name}}</td>
          <td><img :src="coin.image" width="100px"></td>
          <td>{{coin.current_price | toDecimal}}</td>
          <td v-bind:style="getColour(coin.price_change_percentage_1h_in_currency)"><span v-if="coin.price_change_percentage_1h_in_currency > 0">+</span>{{coin.price_change_percentage_1h_in_currency.toFixed(2)}}</td>
          <td v-bind:style="getColour(coin.price_change_percentage_24h)"><span v-if="coin.price_change_percentage_24h > 0">+</span>{{coin.price_change_percentage_24h.toFixed(2)}}</td>
          <td v-bind:style="getColour(coin.price_change_percentage_7d_in_currency)"><span v-if="coin.price_change_percentage_7d_in_currency > 0">+</span>{{coin.price_change_percentage_7d_in_currency.toFixed(2)}}</td>
          <td>{{coin.market_cap | numberFilter}}</td>
        </tr>
      </tbody>

    </table>



  </div>

</div>

  </div>

</template>

<script>


import {eventBus} from '@/main.js'

export default {
  name: 'App',
  data(){
    return {
    url: 'https://api.coingecko.com/api/v3/coins/markets?vs_currency=gbp&order=market_cap_desc&per_page=20&page=1&sparkline=false&price_change_percentage=1h%2C24h%2C7d',
    cryptoData: [],
    allCoins: [],
    selectedCoin: null
  }
  },
  methods: {
    getColour: (num) => num > 0 ? "color:green" : "color:red",
    getCoinData(){
      fetch(this.url)
      .then(res => res.json())
      .then(data => this.cryptoData = data)
    },
    getAllCoins(){
      fetch('https://api.coingecko.com/api/v3/coins/list')
      .then(res => res.json())
      .then(data => this.allCoins = data)
    }
  },
  mounted(){
    this.getCoinData();
    this.getAllCoins();

    eventBus.$on('selected-coin', (coin) => {
      this.selectedCoin = coin
    })
  },
  filters: {
    numberFilter(num) {
      return Number(num).toLocaleString()
    },
    toDecimal(num) {
      return Number(num).toFixed(2);
    }
  }
}
</script>

<style lang="css" scoped>
h3 {
  text-align: center;
  margin: 30px;
  font-weight: bold;
}

td img {
  width: 25px;
}

. jumbotron p {
  font-size: 1.2em;
}

.jumbotron {
  margin-top: 5em;
  margin-bottom: 5em;
}
</style>
