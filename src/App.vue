<template>
  <div id="app">
    <div class="search">
      <input
        type="text"
        class="search_bar"
        placeholder="ingrese ciudad"
        v-model="Input"
        @keypress="Search"
      />
    </div>
    <template> </template>

    <div class="coin-container" v-for="coin in Topcryptos" :key="coin.id">
      <div class="coin-row">
        <div class="coin">
          <img :src="coin.image" :alt="coin.id" />
          <h1>{{ coin.id }}</h1>
          <p class="coin-symbol">{{ coin.symbol }}</p>
        </div>
        <div class="coin-data">
          <p class="coin-price">${{ coin.current_price }}</p>
          <p class="coin-volumen">${{ coin.market_cap.toLocaleString() }}</p>

          <p
            v-if="coin.price_change_percentage_24h < 0"
            class="coin-percent red"
          >
            {{ coin.price_change_percentage_24h.toFixed(2) }}%
          </p>
          <p v-else class="coin-percent green">
            {{ coin.price_change_percentage_24h.toFixed(2) }}%
          </p>

          <p class="coin-marketcap">
            Mkt Cap:{{ coin.market_cap.toLocaleString() }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  components: {},
  data() {
    return {
      Topcryptos: [],
      errors: [],
      Input: "",
      Crypto: {
        name: "",
      },
    };
  },
  methods: {
    Search(e) {
      if (e.key == "Enter") {
        axios
          .get(`https://api.coingecko.com/api/v3/coins/${this.Input}`)
          .then((res) => console.log(res));
      }
    },
  },
  mounted() {
    axios
      .get(
        "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
      )
      .then((res) => {
        this.Topcryptos = res.data;
        console.log(res);
      })
      .catch((errores) => this.errors.push(errores));
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  background-color: #1a1a1c;
  color: #fff;
}
.coin-container {
  display: flex;
  justify-content: center;
}
.coin-row {
  display: flex;
  flex-direction: row;
  justify-items: start;
  align-items: center;
  height: 80px;
  border-bottom: 1px solid #d7d7d7;
  width: 900px;
}
.coin {
  display: flex;
  align-items: center;
  padding-right: 24px;
  min-width: 300px;
}
.coin h1 {
  font-size: 16px;
  width: 150px;
}
.coin img {
  height: 30px;
  width: 30px;
  margin-right: 10px;
}
.coin-symbol {
  text-transform: uppercase;
}
.coin-data {
  display: flex;
  text-align: center;
  justify-content: space-between;
  width: 100%;
}
.coin-price {
  width: 110px;
}
.coin-volume {
  width: 155px;
}
.coin-marketcap {
  width: 230px;
}
.coin-percent {
  width: 100%;
}
.red {
  color: #f00606;
}
.green {
  color: #11d811;
}
</style>
