<template>
  <div class="stats">
    <div>
      <ProgressWheel :percentage="stats.cpu"></ProgressWheel>
      <h3>{{stats.cpu+"%"}}</h3>
      <p> CPU Usage</p>
    </div>
    <div>
      <ProgressWheel :percentage="stats.ram"></ProgressWheel>
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
import ProgressWheel from "./ProgressWheel";
export default {
  name: "Stats",
  components: {ProgressWheel},
  mounted() {
    console.log("mounted")
    this.refreshStats()
  },
  props:{
    connection:String
  },
  data(){
    return{
      websocket:null,
      stats:{}
    }
  },

  computed:{

  },
  methods:{
    getCircleArc:function(percentage){
      var start = this.polarToCartesian(200,200,100,-135)
      var end =this.polarToCartesian(200,200,100,-135+(percentage/100*270))
      var largeArcFlag= percentage/100*270 <=180?"0":"1";
      var d=
        "M "+start.x+" "+start.y +" "+
        "A "+100+" "+100+" "+0+" "+largeArcFlag+" "+1+" "+end.x+" "+end.y

      return d;
    },
    polarToCartesian:function (centerX, centerY, radius, angleInDegrees) {
      var angleInRadians = (angleInDegrees-90) * Math.PI / 180.0;

      return {
        x: centerX + (radius * Math.cos(angleInRadians)),
        y: centerY + (radius * Math.sin(angleInRadians))
      };
    },
    refreshStats:function (){
      let vueInstance=this;
      console.log(this.connection)
      this.websocket= new WebSocket(this.connection)
      this.websocket.onmessage= function(event){
        console.log(event.data);
        vueInstance.stats=JSON.parse(event.data)
      }
    },


  },
  watch:{
    connection:function(){
      console.log("connectionchanged");
      this.refreshStats();
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
    margin:0;
    font-size: 4em;
  }
</style>
