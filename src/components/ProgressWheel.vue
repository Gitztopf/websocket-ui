<template>
  <div class="progress">
    <svg width="400" height="300">

      <path :d="getCircleArc(100)" stroke="rgba(224, 255, 241, 0.75)" stroke-width="30"  fill="none"/>
      <path :d="getCircleArc(percentage)" stroke="rgba(3, 252, 161)" stroke-width="30"  fill="none"/>
    </svg>
  </div>
</template>

<script>
export default {
  name: "ProgressWheel",
  props:{
    percentage:Number
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

</style>
