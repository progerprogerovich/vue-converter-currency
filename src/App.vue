<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <h1>VueJS Currency Exchange App</h1>
    <div class="container">
      <div class="container-one">
        <select
          name="first-currency"
          id="first-currency"
          v-model="currency_one"
        >
          <option value="ARS">ARS</option>
          <option value="AUD">AUD</option>
          <option value="BGN">BGN</option>
          <option value="BRL">BRL</option>
          <option value="BSD">BSD</option>
          <option value="CAD">CAD</option>
          <option value="CHF">CHF</option>
          <option value="CLP">CLP</option>
          <option value="CNY">CNY</option>
          <option value="COP">COP</option>
          <option value="CZK">CZK</option>
          <option value="DKK">DKK</option>
          <option value="DOP">DOP</option>
          <option value="EGP">EGP</option>
          <option value="EUR">EUR</option>
          <option value="FJD">FJD</option>
          <option value="USD">USD</option>
        </select>
        <input
          type="number"
          name="input-one"
          id="input-one"
          v-model="amountOne"
          @input="fetchData()"
        />
      </div>
      <div class="container-two">
        <button @click="switchValues()">Switch</button>
        <h4 id="baseValue">
          1 {{ currency_one }} = {{ rate }} {{ currency_two }}
        </h4>
      </div>
      <div class="container-three">
        <select
          name="second-currency"
          id="second-currency"
          v-model="currency_two"
        >
          <option value="ARS">ARS</option>
          <option value="AUD">AUD</option>
          <option value="BGN">BGN</option>
          <option value="BRL">BRL</option>
          <option value="BSD">BSD</option>
          <option value="CAD">CAD</option>
          <option value="CHF">CHF</option>
          <option value="CLP">CLP</option>
          <option value="CNY">CNY</option>
          <option value="COP">COP</option>
          <option value="CZK">CZK</option>
          <option value="DKK">DKK</option>
          <option value="DOP">DOP</option>
          <option value="EGP">EGP</option>
          <option value="EUR">EUR</option>
          <option value="FJD">FJD</option>
          <option value="USD">USD</option>
        </select>
        <input
          type="number"
          id="amount-two"
          placeholder="0"
          disabled
          v-model="amountTwo"
        />
      </div>
      <div class="container-four">
        <h4 id="lastlyUpdated">
          Lastly Updated: {{ data.time_last_update_utc }}
        </h4>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      data: [],
      currency_one: "USD",
      currency_two: "EUR",
      rate: "",
      amountOne: 1,
      amountTwo: 0,
    };
  },
  methods: {
    fetchData() {
      fetch(
        `https://v6.exchangerate-api.com/v6/e50a6bc6d6c8af42e5a97c11/latest/${this.currency_one}`
      )
        .then((res) => res.json())
        .then((data) => {
          this.data = data;
          this.rate = data.conversion_rates[this.currency_two];
          this.amountTwo = this.amountOne * this.rate.toFixed(2);
        });
    },
    switchValues() {
      const temporaryValue = this.currency_one;
      this.currency_one = this.currency_two;
      this.currency_two = temporaryValue;
      this.fetchData();
    },
  },

  mounted() {
    this.fetchData();
  },
};
</script>

<style>
html {
  background: #f4f4f4f4;
}
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  align-content: center;
  width: 100%;
  height: 100%;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
  color: #5fbaaf;
}

img {
  width: 150px;
}

.container {
  width: 50%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-content: center;
  align-items: center;
  text-align: center;
}

.container-two {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 50%;
}

.container-two button {
  cursor: pointer;
  padding: 5px;
  font-size: 18px;
  background-color: #5fbaaf;
  width: 30%;
  height: 10%;
  border: none;
  outline: none;
}

select {
  padding: 5px;
  margin: 5px;
  border: 1px solid rgba(0, 0, 0, 0.5);
  outline: none;
}

input {
  padding: 5px;
  margin: 5px;
  border: 1px solid rgba(0, 0, 0, 0.5);
  outline: none;
  font-size: 18px;
}

#lastlyUpdated {
  font-weight: 500;
}

#baseValue {
  font-weight: 500;
}
</style>
