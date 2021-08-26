<template>
  <div id="app">
    <h1>Gritape.js Workshop</h1>

    <p>The token info</p>

    <p>{{ tokenInfo }}</p>
    
    <div>
      <p>Your count is: {{count}}</p>
      <button @click="increment()">Increment</button>

    </div>
    </div>
</template>

<script>
import { griptape } from '@stakeordie/griptape.js';
import { stkd } from './contracts'
import { secretCounter } from './contracts/secret-counter'

export default {
  name: 'App',
  data(){
    return{
      tokenInfo: undefined,
      count: 0
    }
  },
  mounted(){
    griptape.onConnect(() => {
        this.fetchTokenInfo()
        this.getCount();
    });
  },
  methods:{
    async fetchTokenInfo(){
      const res = await stkd.getTokenInfo()
      this.tokenInfo = res.token_info
    },
    async increment(){
      await secretCounter.doIncrement();
      await this.getCount();
    },
    async getCount(){
      const res = await secretCounter.getCount();
      this.count = res.count;
    }
  }

}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
