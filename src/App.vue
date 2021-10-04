<template>
  <div id="app">
    <Stats port="8765"></Stats>
  </div>
</template>

<script>

import Stats from "@/components/Stats";

export default {
  name: 'App',
  data(){
  },
  components:{
    Stats
  },
  created() {
    this.refreshStats()
  },
  methods:{

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
