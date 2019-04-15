<template>
  <div id="ColorPicker">
    <div id="brightnessSaturationSelect">

    </div>
    <div id="ColorSelect" @click="getClickCoordinate">

    </div>
    <div id="ColorResult">

    </div>
    {{ clickColorCoordinateY }}
  </div>
</template>

<script>
export default {
  name: 'ColorPicker',
  data () {
    return {
      clickColorCoordinateY : '',
    }
  },
  computed: {
    }
  },
  methods: {
    getClickCoordinate: function(e){
      this.clickColorCoordinateY = e.offsetY
    },
    detectR: function(e){
      let R
      let clickColorCoordinate = e.offsetY
      let degree = clickColorCoordinate / 300 * 360
      if(degree < 60){
        R = 255
      }else if( 60 <= degre && degree < 120){
        
      }else if( 60 <= degre && degree < 120){
        
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #brightnessSaturationSelect{
    width: 300px;
    height: 300px;
    background-image: linear-gradient(to bottom, transparent, #000000 ), linear-gradient(to right, #FFFFFF, transparent);
    background-color: red;
  }

  #ColorSelect{
    width: 100px;
    height: 300px;
    background-image: linear-gradient( to bottom, rgb(255,0,0), rgb(255,255,0), rgb(0,255,0), rgb(0,255,255), rgb(0,0,255), rgb(255,0,255), rgb(255,0,0) );
  }
</style>
