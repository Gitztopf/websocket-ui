<template>
  <div class="stats">
    <div>
      <h3>{{stats.cpu+"%"}}</h3>
      <p> CPU Usage</p>
    </div>
    <div>
      <h3>
        {{stats.ram+"%"}}
      </h3>
      <p> Ram Usage</p>
    </div>
    <div>
      <h3>
        {{stats.battery+"%"}}
      </h3>
      <p>Battery Percentage</p>
    </div>
  </div>

</template>

<script>
export default {
  name: "Stats",
  created() {
    this.refreshStats()
  },
  props:{
    port:Number
  },
  data(){
    return{
      websocket:null,
      stats:{}
    }
  },
  methods:{
    refreshStats:function (){
      let vueInstance=this;
      this.websocket= new WebSocket("ws://127.0.0.1:"+this.port)
      this.websocket.onmessage= function(event){
        console.log(event.data);
        vueInstance.stats=JSON.parse(event.data)
      }
    }

  }
}
</script>

<style scoped>
  .stats{
    display: flex;
    justify-content: space-evenly;
    flex-wrap: wrap;
    align-items: center;
  }
  h3{
    font-size: 4em;
  }
</style>