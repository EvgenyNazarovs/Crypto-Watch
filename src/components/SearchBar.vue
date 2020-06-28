<template lang="html">
  <form v-on:submit.prevent>
      <input type="text"
             v-model="search"
             placeholder="enter coin"
             v-on:keyup="searchForCoin"
             >
      <select v-on:change="onSelect" v-model="selectedCoin">
        <option disabled value="">Select a coin</option>
        <option v-for="coin in filteredCoins" :value="coin">{{coin.name}}</option>
    </select>
  </form>

</template>


<script>

import {eventBus} from '@/main.js'

export default {
  name: 'SearchBar',
  props: ['allCoins'],
  data(){
    return {
      "search": "",
      "selectedCoin": {},
      "filteredCoins": []
    }
  },
  methods: {
    searchForCoin(){
      filteredCoins =  allCoins.filter(coin => coin.name.includes(this.search))
    },
    onSelect(){
      this.search = ""
      eventBus.$emit('selected-coin', this.selectedCoin)
    }
    }
  }
</script>

<style lang="css" scoped>
form {
  text-align: center;
  margin: 40px 0;
}

select, input[type="text"]{
  font-size: 18px;
}

select {
  margin-left: 20px;
}
</style>
