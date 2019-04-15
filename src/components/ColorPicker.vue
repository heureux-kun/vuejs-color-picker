<template>
  <div id="ColorPicker">
    <div id="brightnessSaturationSelect" v-bind:style="colorPickerBackgroundColor">

    </div>
    <div id="ColorSelect" @click="getClickCoordinate">

    </div>
    <div id="ColorResult" @click="selectColor">

    </div>
    clickColorCoordinateY:{{ clickColorCoordinateY }}<br />
    degree:{{ degree }}<br />
    {{ rColor }},{{ gColor }},{{ bColor }}
  </div>
</template>

<script>
export default {
  name: 'ColorPicker',
  data () {
    return {
      clickColorCoordinateY : '',
      degree: '',
      rColor: '',
      gColor: '',
      bColor: ''
    }
  },
  computed: {
    colorPickerBackgroundColor: function(){
      return 'background-color:rgb('+ this.rColor + ','+this.gColor + ','+ this.bColor + ')'
    }
  },
  methods: {
    getClickCoordinate: function(e){
      this.clickColorCoordinateY = e.offsetY
      this.degree = this.clickColorCoordinateY / 300 * 360
      //R
      if(this.degree < 60){
        this.rColor = 255
        this.gColor = Math.round( this.degree * (255 / 60) )
        this.bColor = 0
      }else if( 60 <= this.degree && this.degree < 120){
        this.rColor = Math.round( 255 - ((this.degree - 60) * 255 / 60) )
        this.gColor = 255
        this.bColor = 0
      }else if( 120 <= this.degree && this.degree < 180){
        this.rColor = 0
        this.gColor = 255
        this.bColor = Math.round( (this.degree - 120) * 255 / 60 )
      }else if( 180 <= this.degree && this.degree < 240){
        this.rColor = 0
        this.gColor = Math.round( 255 - (( this.degree-180 ) * 255 / 60) )
        this.bColor = 255
      }else if( 240 <= this.degree && this.degree < 300){
        this.rColor = Math.round( (this.degree - 240) * 255 / 60 )
        this.gColor = 0
        this.bColor = Math.round( 255 - ((this.degree - 240) * 255 / 60) )
      }else if( 300 <= this.degree && this.degree < 360){
        this.rColor = 255
        this.gColor = 0
        this.bColor = 0
      }
    },
    selectColor: function(){
      //ここから
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
  }

  #ColorSelect{
    width: 100px;
    height: 300px;
    background-image: linear-gradient( to bottom, rgb(255,0,0), rgb(255,255,0), rgb(0,255,0), rgb(0,255,255), rgb(0,0,255), rgb(255,0,255), rgb(255,0,0) );
  }
</style>
