<template>
  <div class="exchange-rate" id="app">
    <h1>Live Bitcoin Price Index</h1>
    <h5>provided from CoinDesk.com</h5>
    <div v-for="(currency, key) in info" :key="key">
      {{ currency.description }}:
      <span>
        <span v-html="currency.symbol"></span
        >{{ currency.rate_float | currencydecimal }}
      </span>
    </div>
    <br />
    <h3>How much USD do you want to turn into bitcoin?</h3>
    <br />
    <b-input-group
      style="width: 50%; margin: 0 auto;"
      size="sm"
      prepend="$"
      append=".00"
    >
      <b-form-input v-model="inputVal"></b-form-input>
    </b-input-group>
    <br />
    <h2>You can buy this much bitcoin!</h2>
    <p>{{ calculate }}</p>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
    info: null,
    inputVal: "",
    bitcoin: 10000,
  }),

  computed: {
    calculate() {
      return this.inputVal / this.bitcoin;
    },
  },

  mounted() {
    axios
      .get("https://api.coindesk.com/v1/bpi/currentprice.json")
      .then((response) => (this.info = response.data.bpi));
  },

  filters: {
    currencydecimal(value) {
      return value.toFixed(2);
    },
  },
};
</script>

<style></style>
