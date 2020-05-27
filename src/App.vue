<template>
  <div id="app">
    <h1 class="main-title">Fuzzy Trader</h1>
    <div class="main-container">
      <Stocks v-if="this.isLogged" />
      <Login v-else :parentData="this.isLogged" v-on:login="checkLogin"/>
    </div>
  </div>
</template>

<script>
import Stocks from "./components/Stocks/Stocks.vue";
import Login from "./components/Login.vue";

export default {
  name: "App",
  components: {
    Stocks,
    Login
  },
  data: function() {
    return {
      isLogged: null
    }
  },
  methods: {
    checkLogin() {
      if (localStorage.getItem("username") != null) {
        this.isLogged = true;
      } else {
        this.isLogged = false;
      }
    }
  },
  created: function(){
    this.checkLogin();
  } 
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans:wght@400;700&family=Oswald:wght@400;500;600;700&display=swap");

*,
*::before,
*::after {
  box-sizing: border-box;
}

template {
  height: 100%;
}

#app {
  font-family: Noto Sans, Helvetica, sans-serif;
  margin: 0;
  height: 100%;
}

.main-title {
  font-family: Oswald, Helvetica, sans-serif;
  font-weight: bold;
  font-size: 4rem;
  color: #05386B;
  text-align: center;
}

.main-container {
  width: 80%;
  height: 100%;
  background-image: linear-gradient(to bottom right, #5CDB95, #379683);
  border-radius: 16px;
  box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.5);
  padding: 32px;
  padding-top: 8px;
  text-align: center;
  margin-left: auto;
  margin-right: auto;
}
</style>
