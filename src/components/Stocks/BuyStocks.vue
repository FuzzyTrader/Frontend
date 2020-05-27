<template>
  <div>
    <h2>Quanto você quer investir?</h2>
    <div class="investment">
      <h2>USD</h2>
      <input v-model="value" />
      <button class="update-stock-prices" @click="this.calculateStocksToBuy">Ok</button>
    </div>
    <div class="stocks">
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">{{ this.stocks[0].symbol }}</h2>
        <h1 class="stocks__card-price">{{ this.stocks[0].price }}</h1>
        <h2 class="stocks__card-amount">x {{ this.stocks[0].amount }}</h2>
        <h3 class="stocks__card-total">USD {{ this.stocks[0].totalPrice }}</h3>
      </div>
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">{{ this.stocks[1].symbol }}</h2>
        <h1 class="stocks__card-price">{{ this.stocks[1].price }}</h1>
        <h2 class="stocks__card-amount">x {{ this.stocks[1].amount }}</h2>
        <h3 class="stocks__card-total">USD {{ this.stocks[1].totalPrice }}</h3>
      </div>
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">{{ this.stocks[2].symbol }}</h2>
        <h1 class="stocks__card-price">{{ this.stocks[2].price }}</h1>
        <h2 class="stocks__card-amount">x {{ this.stocks[2].amount }}</h2>
        <h3 class="stocks__card-total">USD {{ this.stocks[2].totalPrice }}</h3>
      </div>
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">{{ this.stocks[3].symbol }}</h2>
        <h1 class="stocks__card-price">{{ this.stocks[3].price }}</h1>
        <h2 class="stocks__card-amount">x {{ this.stocks[3].amount }}</h2>
        <h3 class="stocks__card-total">USD {{ this.stocks[3].totalPrice }}</h3>
      </div>
    </div>
    <h1 id="total">Total: USD {{ this.totalPrice }}</h1>
    <button @click="this.buyStocks" :disabled="this.value <= 0">Comprar ativos</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "BuyStocks",
  data: function() {
    return {
      value: 0,
      stocks: [{
        symbol    : "IBM",
        price     : 0,
        amount    : 0,
        totalPrice: 0
      }, {
        symbol    : "DELL",
        price     : 0,
        amount    : 0,
        totalPrice: 0
      }, {
        symbol    : "DPZ",
        price     : 0,
        amount    : 0,
        totalPrice: 0
      }, {
        symbol    : "NKE",
        price     : 0,
        amount    : 0,
        totalPrice: 0
      }],
      totalPrice : 0,
    }
  },
  methods: {
    calculateStocksToBuy() {
      let context = this;
      let promises = [];
      for (let i = 0; i < this.stocks.length; i++) {
        promises.push(
          axios.get("https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol=" + this.stocks[i].symbol + "&apikey=RVQEMEEB6QV44GQS")
            .then(response => {
              // The API allows to make 5 requests a minute, this avoids error on console for
              // trying to read undefined value
              if(response.data["Global Quote"] != undefined) {
                let price = response.data["Global Quote"]["05. price"];
                context.stocks[i].price = parseFloat(price).toFixed(2);
              }
            })
        );
      }

      Promise.all(promises)
        .then(() => {
          let stocks = context.stocks;
          let sum = parseFloat(stocks[0].price) + parseFloat(stocks[1].price) + parseFloat(stocks[2].price) + parseFloat(stocks[3].price);

          let equalAmount = (context.value)/sum;
          let reminding = parseInt((context.value)%sum);
          let amount = 0;

          context.totalPrice = 0;
          for(let i = 0; i < stocks.length; i++) {
            amount = 0;
            if(reminding >= stocks[i].price) {
              amount = parseInt(reminding/(stocks[i].price));
              reminding = reminding%(stocks[i].price);
            }
            stocks[i].amount = parseInt(equalAmount + amount);
            stocks[i].totalPrice = stocks[i].amount * stocks[i].price;
            context.totalPrice += stocks[i].totalPrice;

            // Using toFixed to fix presentation of the total stock price
            stocks[i].totalPrice = parseFloat(stocks[i].totalPrice).toFixed(2);
          }
          context.totalPrice = parseFloat(context.totalPrice).toFixed(2);
        });
    },
    buyStocks() {
      let stocks = []
      for(let i = 0; i < this.stocks.length; i++) {
        stocks.push({
          "code"   : this.stocks[i].symbol,
          "amount" : this.stocks[i].amount
        })
      }

      let postData = {
        "stocks"   : stocks,
        "username" : localStorage.getItem("username")
      }
      let context = this;
      axios.post("https://desolate-spire-14577.herokuapp.com/api/stocks/add", postData)
        .then(() => {
          alert("Ativos comprados com sucesso!");
          context.$emit("displayWallet", context.buyStocksView);
        });
    }
  },
};
</script>

<style>
h1,
h2,
h3 {
  font-weight: 400;
}

.welcome {
  text-align: left;
}

.investment {
  display: flex;
  justify-content: center;
  align-items: center;
}

input {
  margin: 0 8px;
  padding: 16px;
  width: 8rem;
  border-radius: 0;
  border: 2px solid #05386B;
  background-color: #F6F6F6;
  font-size: 1.275rem;
  text-align: center;
}

input:focus {
  border: 3px solid #05386B;
}

.stocks {
  display: flex;
  justify-content: center;
}

.stocks__card {
  background-color: #F6F6F6;
  border: 3px solid #05386B;
  margin: 16px;
  width: 20%;
  padding: 0 8px;
}

.stocks__card-symbol,
.stocks__card-total {
  background-color: #05386B;
  height: 15%;
  color: #EDF5E1;
  display: flex;
  justify-content: center;
  align-items: center;
}

button {
  display: flex;
  margin-left: auto;
  background-color: #05386B;
  border: 3px solid #05386B;
  padding: 16px;
  border-radius: 8px;
  cursor: pointer;
  text-transform: uppercase;
  font-weight: bold;
  color: #F6F6F6;
}

button:hover,
button:active {
  background-color: transparent;
  color: #05386B;
}

.update-stock-prices {
  border-radius: 50%;
  margin-left: 8px;
}

</style>
