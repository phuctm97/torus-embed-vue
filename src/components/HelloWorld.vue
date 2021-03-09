<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p v-if="!account">
      You didn't logged in.
      <button @click="login">Login</button>
    </p>
    <div class="info-container" v-if="account">
      <h3>Your account information</h3>
      <div class="info">
        <p><strong>Address</strong>: {{ account.address }}</p>
        <p><strong>Balance</strong>: {{ account.balance }}</p>
      </div>
    </div>
    <!-- <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul> -->
  </div>
</template>

<script>
import Torus from "@toruslabs/torus-embed";
import Web3 from "web3";

export default {
  name: "HelloWorld",
  data: function() {
    return {
      account: null,
    };
  },
  props: {
    msg: String,
  },
  methods: {
    async login() {
      const torus = new Torus();
      await torus.init();
      await torus.login({ enableLogging: false });

      const web3 = new Web3(torus.provider);
      const address = (await web3.eth.getAccounts())[0];
      const balance = await web3.eth.getBalance(address);
      this.account = {
        address,
        balance,
      };
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.info-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.info {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: flex-start;
  margin-top: 20px;
}
.info p {
  margin-bottom: 4px;
  margin-top: 0;
}
</style>
