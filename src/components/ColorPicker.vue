<template>
  <div id="ColorPicker">
    <div id="SelectPoint" v-bind:style="pointPosition"></div>
    <div id="BrightnessSaturationSelect" v-bind:style="colorPickerBackgroundColor" @click="getSv">

    </div>
    H：{{h}}° S：{{s}}% V：{{v}}%　R：{{r}}　G：{{g}}　B：{{b}}
    <div id="ColorSelect" @click="getClickCoordinate">

    </div>
    <div id="ColorResult">
      
    </div>
    clickColorCoordinateY:{{ clickColorCoordinateY }}<br />
    degree:{{ degree }}<br />
    rgb({{ rColor }},{{ gColor }},{{ bColor }})
  </div>
</template>

<script>
export default {
  name: 'ColorPicker',
  data () {
    return {
      clickColorCoordinateY : '',
      coordinateX: '',
      coordinateY: '',
      degree: '360',
      rColor: '-',
      gColor: '-',
      bColor: '-',
      h: '',
      s: '0',
      v: '100',
      r: '-',
      g: '-',
      b: '-'
    }
  },
  computed: {
    colorPickerBackgroundColor: function(){
      return 'background-color:rgb('+ this.rColor + ','+this.gColor + ','+ this.bColor + ')'
    },
    pointPosition: function(){
      let x = this.coordinateX - 6
      let y = this.coordinateY + 6
      return 'left: ' + x + 'px;'+'top: ' + y + 'px;'
    },
    hsvToRgb: function() {
      let H = this.h
      let S = this.s
      let V = this.v
      let C = V * S;
      let Hp = H / 60;
      let X = C * (1 - Math.abs(Hp % 2 - 1));

      let R, G, B;
      if (0 <= Hp && Hp < 1) {[R,G,B]=[C,X,0]};
      if (1 <= Hp && Hp < 2) {[R,G,B]=[X,C,0]};
      if (2 <= Hp && Hp < 3) {[R,G,B]=[0,C,X]};
      if (3 <= Hp && Hp < 4) {[R,G,B]=[0,X,C]};
      if (4 <= Hp && Hp < 5) {[R,G,B]=[X,0,C]};
      if (5 <= Hp && Hp < 6) {[R,G,B]=[C,0,X]};

      let m = V - C;
      [R, G, B] = [R+m, G+m, B+m];

      R = Math.floor(R * 255);
      G = Math.floor(G * 255);
      B = Math.floor(B * 255);

      this.r = R
      this.g = G
      this.b = B
    }
  },
  methods: {
    getClickCoordinate: function(e){
      this.clickColorCoordinateY = e.offsetY
      this.degree = 360 - Math.floor(this.clickColorCoordinateY / 300 * 360)
      this.h = this.degree
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
    getSv: function(e){
      this.coordinateX = e.offsetX
      this.coordinateY = e.offsetY
      this.s = Math.floor(this.coordinateX / 300 * 100)
      this.v = 100 - Math.floor(this.coordinateY / 300 * 100)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #SelectPoint{
    width: 12px;
    height: 12px;
    border-radius: 12px;
    border:1px solid #fff;
    position: relative;
  }
  #BrightnessSaturationSelect{
    cursor: crosshair;
    width: 300px;
    height: 300px;
    background-image: linear-gradient(to bottom, transparent, #000000 ), linear-gradient(to right, #FFFFFF, transparent);
  }
  #ColorSelect{
    width: 100px;
    height: 300px;
    background-image: linear-gradient( to top, rgb(255,0,0), rgb(255,255,0), rgb(0,255,0), rgb(0,255,255), rgb(0,0,255), rgb(255,0,255), rgb(255,0,0) );
  }
</style>
