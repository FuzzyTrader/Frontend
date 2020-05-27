<template>
  <div>
    <h1 class="welcome">Bem vindo!</h1>
    <h2>Insira seus dados para realizar o cadastro ou faça o login com os dados já existentes.</h2>
    <form>    
        <h2>Nome de usuário</h2>
        <input v-model="username">
        <h2>Senha</h2>
        <input v-model="password" type="password">
    </form>
    <div class="buttons">
        <button @click="this.register">Realizar cadastro</button>
        <button @click="this.login">Login</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "Login",
  data: function() {
    return {
      username: '',
      password: '',
      isLogged: false
    }
  }, 
  methods: {
    register() {
        let formData = {
          "username" : this.username,
          "password" : this.password
        }
        let context = this;
        axios.post("https://desolate-spire-14577.herokuapp.com/api/users/register", formData)
          .then((response) =>{
            if(response.status == 200) {
                alert("Usuário criado! Realize o login para acessar o sistema");
                context.username = "";
                context.password = "";              
            }
          });
    },
    login() {
        let formData = {
          "username" : this.username,
          "password" : this.password
        }
        let context = this;
        axios.post("https://desolate-spire-14577.herokuapp.com/api/users/login", formData)
          .then((response) => {
            if(response.status == 200) {
                localStorage.setItem("username", context.username);
                console.log(response);
                localStorage.setItem("token", response.data.data.token);
                context.isLogged = true;
                context.$emit("login", context.isLogged);
            }
          });
    }
  }
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

form {
  max-width: 80%;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  margin: auto;
}

input {
  padding: 8px;
  width: 20rem;
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

.buttons {
  display: flex;
  align-items: center;
  justify-content: center;
}

button {
  display: flex;
  margin: 0 16px;
  margin-top: 48px;
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

</style>
