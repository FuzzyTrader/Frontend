<template>
    <div>
        <h3 v-if="!buyStocksView" @click="displayBuyStocks" class="change-view">Fazer investimentos »</h3>
        <h3 v-if="(hasWallet && buyStocksView)" @click="displayWallet" class="change-view">Ver carteira »</h3>
        <h1 class="welcome">Olá, {{this.username}}!</h1>
        <BuyStocks v-if="buyStocksView" :parentData="this.buyStocksView" v-on:displayWallet="displayWallet"/>
        <Wallet v-if="!buyStocksView" :parentData="this.buyStocksView" v-on:displayWallet="displayWallet"/>
    </div>
</template>

<script>
import axios from 'axios';
import BuyStocks from "./BuyStocks.vue";
import Wallet from "./Wallet.vue";

export default {
    name: "Stocks",
    components: {
        BuyStocks,
        Wallet
    },
    data: function() {
        return {
            username: "usuário",
            hasWallet: false,
            buyStocksView: true
        }
    },
    methods: {
        displayWallet() {
            this.buyStocksView = false;
        },
        displayBuyStocks() {
            this.buyStocksView = true;
        }
        },
    created: function() {
        axios.defaults.headers.common['Authorization'] = 'Bearer ' + localStorage.getItem("token");
        this.username = localStorage.getItem("username");

        let context = this;

        try {
            axios.get("https://desolate-spire-14577.herokuapp.com/api/stocks/wallet?username=" + this.username)
                .then(() => {
                    context.hasWallet = true;
                });
        } catch (e) {
            context.hasWallet = false;
        }

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

.change-view {
    text-align: right;
    text-transform: uppercase;
    font-weight: bold;
    cursor: pointer;
    color: #F6F6F6;
}

.change-view:hover,
.change-view:active {
    color: #05386B;
}

</style>
