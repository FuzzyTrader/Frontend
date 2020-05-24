<template>
  <div>
    <h1 class="welcome">Olá, usuário!</h1>
    <h2>Quanto você quer investir?</h2>
    <div class="investment">
      <h2>USD</h2>
      <input type="text" id="value" />
    </div>
    <div class="stocks">
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">ABC</h2>
        <h1 class="stocks__card-price">17.24</h1>
        <h2 class="stocks__card-amount">x 5</h2>
        <h3 class="stocks__card-total">USD 86.20</h3>
      </div>
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">ABC</h2>
        <h1 class="stocks__card-price">17.24</h1>
        <h2 class="stocks__card-amount">x 5</h2>
        <h3 class="stocks__card-total">USD 86.20</h3>
      </div>
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">ABC</h2>
        <h1 class="stocks__card-price">17.24</h1>
        <h2 class="stocks__card-amount">x 5</h2>
        <h3 class="stocks__card-total">USD 86.20</h3>
      </div>
      <div class="stocks__card">
        <h2 class="stocks__card-symbol">ABC</h2>
        <h1 class="stocks__card-price">17.24</h1>
        <h2 class="stocks__card-amount">x 5</h2>
        <h3 class="stocks__card-total">USD 86.20</h3>
      </div>
    </div>
    <h1 id="total">Total: USD 431.00</h1>
    <button id="buy-stocks">Comprar ativos</button>
  </div>
</template>

<script>
export default {
  name: "Stocks",
};
</script>

<style scoped>
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
  background-color: #EDF5E1;
  font-size: 1.275rem;
}

input:focus {
  border: 3px solid #05386B;
}

.stocks {
  display: flex;
  justify-content: center;
}

.stocks__card {
  background-color: #EDF5E1;
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
  color: #EDF5E1;
}

button:hover,
button:active {
  background-color: transparent;
  color: #05386B;
}

</style>
