<script>
export default {
  name: 'App',
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ['#', 'Coin', 'Price', 'Price Change', '24 Volume'],
      textSearch: '',
      loading: false,
    };
  },
  async mounted() {
    this.loading = true;
    const res = await fetch(
      'https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1%sparklline=false'
    );
    const data = await res.json();
    this.coins = data;
    this.filteredCoins = data;
    this.loading = false;
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
};
</script>
<template lang="">
  <div class="container">
    <div v-if="loading" class="row">Loading...</div>
    <div class="row" v-if="!loading">
      <input
        type="text"
        class="form-control text-light bg-dark rounded-0 border-0 my-4"
        placeholder="Search Coin"
        @keyup="searchCoin"
        v-model="textSearch" />
      <table class="table table-dark">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">{{ title }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" ::key="coin.id">
            <td>{{ index + 1 }}</td>
            <td>
              <img :src="coin.image" style="width: 2rem" class="me-2" />
              <span>{{ coin.name }}</span>
              <span class="ms-2 text-uppercase text-muted">{{ coin.symbol }}</span>
            </td>
            <td>${{ coin.current_price }}</td>
            <td :class="coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger'">
              {{ coin.price_change_percentage_24h }}%
            </td>
            <td>${{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
