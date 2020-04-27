<template lang="html">
  <form v-on:submit.prevent>
      <input type="text"
             v-model="search"
             placeholder="search for a specific coin"
             v-on:keyup="searchForCoin"
             >
      <select v-on:change="onSelect" v-model="selectedCoin">
        <option disabled value="">Select a coin</option>
        <option v-for="coin in allCoins" :value="coin">{{coin.name}}</option>
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
      "selectedCoin": {}
    }
  },
  methods: {
    searchForCoin(){
      let foundCoin = this.allCoins.find((coin) => {
        return coin.name.toLowerCase().indexOf(this.search.toLowerCase()) -1
      })
      this.selectedCoin = foundCoin

      eventBus.$emit('selected-coin', this.selectedCoin)
    },
    onSelect(){
      this.search = ""
      eventBus.$emit('selected-coin', this.selectedCoin)
    }
    }
  }
</script>

<style lang="css" scoped>
</style>
