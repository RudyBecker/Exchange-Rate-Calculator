<template>
  <div class="exchange-rate" id="app">
    <h1>Live Bitcoin Price Index</h1>
    <h5>provided from CoinDesk.com</h5>
    <div>
      <span></span>
    </div>
    <div class="currency-selector">
      <b-form-select v-model="selected">
        <b-form-select-option :value="null" disabled>Select Currency</b-form-select-option>
        <b-form-select-option v-for="(currency, key) in info" :key="key" :value="key">
          {{ currency.description }}:
          <span v-html="currency.symbol"></span>
          {{ currency.rate_float | currencydecimal }}
        </b-form-select-option>
      </b-form-select>
    </div>
    <br />
    <h3>How much {{info[selected] ? info[selected].description : ""}} do you want to turn into bitcoin?</h3>
    <br />
    <b-input-group
      style="width: 50%; margin: 0 auto;"
      size="sm"
      :prepend-html="info[selected]? info[selected].symbol : ''"
    >
      <b-form-input v-model="inputVal"></b-form-input>
    </b-input-group>
    <br />
    <h2>You can buy this much bitcoin!</h2>
    <p>{{ calculate }}</p>
    <br />
    <p>Coded by Rudy Becker</p>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
    info: null,
    inputVal: "",
    selected: null
  }),

  computed: {
    calculate() {
      let rate = this.info[this.selected]
        ? this.info[this.selected].rate_float
        : "";
      return this.inputVal / rate;
    }
  },

  mounted() {
    axios
      .get("https://api.coindesk.com/v1/bpi/currentprice.json")
      .then(response => (this.info = response.data.bpi));
  },

  filters: {
    currencydecimal(value) {
      return value.toFixed(2);
    }
  }
};
</script>

<style>
.currency-selector {
  width: 50%;
  margin: 0 auto;
}
</style>
