<template>
  <div>
    <h2>Esta é a sua carteira consolidada:</h2>
    <div class="stocks">
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">{{ this.stocks["IBM"].symbol }}</h2>
        <h1 class="stocks__card-price">{{ this.stocks["IBM"].price }}</h1>
        <h2 class="stocks__card-amount">x {{ this.stocks["IBM"].amount }}</h2>
        <h3 class="stocks__card-total">USD {{ this.stocks["IBM"].totalPrice }}</h3>
      </div>
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">{{ this.stocks["DELL"].symbol }}</h2>
        <h1 class="stocks__card-price">{{ this.stocks["DELL"].price }}</h1>
        <h2 class="stocks__card-amount">x {{ this.stocks["DELL"].amount }}</h2>
        <h3 class="stocks__card-total">USD {{ this.stocks["DELL"].totalPrice }}</h3>
      </div>
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">{{ this.stocks["DPZ"].symbol }}</h2>
        <h1 class="stocks__card-price">{{ this.stocks["DPZ"].price }}</h1>
        <h2 class="stocks__card-amount">x {{ this.stocks["DPZ"].amount }}</h2>
        <h3 class="stocks__card-total">USD {{ this.stocks["DPZ"].totalPrice }}</h3>
      </div>
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">{{ this.stocks["NKE"].symbol }}</h2>
        <h1 class="stocks__card-price">{{ this.stocks["NKE"].price }}</h1>
        <h2 class="stocks__card-amount">x {{ this.stocks["NKE"].amount }}</h2>
        <h3 class="stocks__card-total">USD {{ this.stocks["NKE"].totalPrice }}</h3>
      </div>
    </div>
    <h1 id="total">Total investido: USD {{ this.totalPrice }}</h1>
  </div>
</template>

<script>
import axios from 'axios';

export default {
    name: "Wallet",
    data: function() {
        return {
            stocks: {
                "IBM" : {
                    symbol    : "IBM",
                    price     : 0,
                    amount    : 0,
                    totalPrice: 0
                }, 
                "DELL" : {
                    symbol    : "DELL",
                    price     : 0,
                    amount    : 0,
                    totalPrice: 0
                },
                "DPZ": {
                    symbol    : "DPZ",
                    price     : 0,
                    amount    : 0,
                    totalPrice: 0
                },
                "NKE": {
                    symbol    : "NKE",
                    price     : 0,
                    amount    : 0,
                    totalPrice: 0
            }},
            totalPrice : 0,
        }
    },
    methods: {
        consolidateWallet() {
            let context = this;
            let promises = [];

            axios.get("https://desolate-spire-14577.herokuapp.com/api/stocks/wallet?username=" + localStorage.getItem("username"))
                .then((response) => {
                    let stocks = response.data.data;
                    let sum = 0;

                    for(let i = 0; i < stocks.length; i++) {
                        promises.push(
                            axios.get("https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol=" + stocks[i].code + "&apikey=RVQEMEEB6QV44GQS")
                                .then(response => {
                                    // The API allows to make 5 requests a minute, this avoids error on console for
                                    // trying to read undefined value
                                    if(response.data["Global Quote"] != undefined) {
                                        let price = response.data["Global Quote"]["05. price"];
                                        context.stocks[stocks[i].code].price = parseFloat(price).toFixed(2);
                                        context.stocks[stocks[i].code].amount = stocks[i].amount;
                                        context.stocks[stocks[i].code].totalPrice = (stocks[i].amount * parseFloat(price)).toFixed(2);
                                        sum += stocks[i].amount * parseFloat(price);
                                        context.totalPrice = parseFloat(sum).toFixed(2);
                                    }
                                })
                        );
                    }
                });
        },
    },
    created: function() {
        this.consolidateWallet();
    }
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
