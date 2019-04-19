<template>
  <div id="ColorPicker">
    <div id="SvAreaSelectPoint" v-bind:style="svAreaCursorPosition"></div>

    <div id="SvArea" v-bind:style="setSvAreaBackgroundColor" @click="getSv">

    </div>
    #{{ hexCode }}<br />
    H：{{hsv.h}}° S：{{hsv.s}}% V：{{hsv.v}}% R：{{rgb.r}} G：{{rgb.g}} B：{{rgb.b}}
    <div id="HArea" @click="getH">

    </div>
    hAreaCoordinateY:{{ hAreaCoordinateY }}<br />
    degree:{{ hsv.h }}<br />
    highBrightnessRgb: {{ highBrightnessRgb.r }},{{ highBrightnessRgb.g }},{{ highBrightnessRgb.b }}
  </div>
</template>

<script>
export default {
  name: 'ColorPicker',
  data () {
    return {
      hAreaCoordinateY: '',
      svAreaCoordinateX: '0',
      svAreaCoordinateY: '0',
      hsv: {
        h: '360',
        s: '0',
        v: '100'
      },
      rgb: {
        r: '-',
        g: '-',
        b: '-'
      },
      highBrightnessRgb: {
        r: '-',
        g: '-',
        b: '-'
      },
      hexCode: ''
    }
  },
  computed: {
    setSvAreaBackgroundColor: function () {
      return 'background-color:rgb(' + this.highBrightnessRgb.r + ',' + this.highBrightnessRgb.g + ',' + this.highBrightnessRgb.b + ')'
    },
    svAreaCursorPosition: function () {
      let x = this.svAreaCoordinateX - 6
      let y = this.svAreaCoordinateY + 6
      return 'left: ' + x + 'px;' + 'top: ' + y + 'px;'
    }
  },
  methods: {
    getH: function (e) {
      this.hAreaCoordinateY = e.offsetY
      this.hsv.h = 360 - Math.floor(this.hAreaCoordinateY / 300 * 360)
      // highBrightnessRgb
      if (this.hsv.h < 60) {
        this.highBrightnessRgb.r = 255
        this.highBrightnessRgb.g = Math.round(this.hsv.h * (255 / 60))
        this.highBrightnessRgb.b = 0
      } else if (this.hsv.h >= 60 && this.hsv.h < 120) {
        this.highBrightnessRgb.r = Math.round(255 - ((this.hsv.h - 60) * 255 / 60))
        this.highBrightnessRgb.g = 255
        this.highBrightnessRgb.b = 0
      } else if (this.hsv.h >= 120 && this.hsv.h < 180) {
        this.highBrightnessRgb.r = 0
        this.highBrightnessRgb.g = 255
        this.highBrightnessRgb.b = Math.round((this.hsv.h - 120) * 255 / 60)
      } else if (this.hsv.h >= 180 && this.hsv.h < 240) {
        this.highBrightnessRgb.r = 0
        this.highBrightnessRgb.g = Math.round(255 - ((this.hsv.h - 180) * 255 / 60))
        this.highBrightnessRgb.b = 255
      } else if (this.hsv.h >= 240 && this.hsv.h < 300) {
        this.highBrightnessRgb.r = Math.round((this.hsv.h - 240) * 255 / 60)
        this.highBrightnessRgb.g = 0
        this.highBrightnessRgb.b = Math.round(255 - ((this.hsv.h - 240) * 255 / 60))
      } else if (this.hsv.h >= 300 && this.hsv.h < 360) {
        this.highBrightnessRgb.r = 255
        this.highBrightnessRgb.g = 0
        this.highBrightnessRgb.b = 0
      }
    },
    getSv: function (e) {
      this.svAreaCoordinateX = e.offsetX
      this.svAreaCoordinateY = e.offsetY
      this.hsv.s = Math.floor(this.svAreaCoordinateX / 300 * 100)
      this.hsv.v = 100 - Math.floor(this.svAreaCoordinateY / 300 * 100)

      // hsvToRgb
      let H = this.hsv.h
      let S = this.hsv.s / 100
      let V = this.hsv.v / 100
      let C = V * S
      let Hp = H / 60
      let X = C * (1 - Math.abs(Hp % 2 - 1))

      let R, G, B
      if (Hp >= 0 && Hp < 1) { [R, G, B] = [C, X, 0] };
      if (Hp >= 1 && Hp < 2) { [R, G, B] = [X, C, 0] };
      if (Hp >= 2 && Hp < 3) { [R, G, B] = [0, C, X] };
      if (Hp >= 3 && Hp < 4) { [R, G, B] = [0, X, C] };
      if (Hp >= 4 && Hp < 5) { [R, G, B] = [X, 0, C] };
      if (Hp >= 5 && Hp <= 6) { [R, G, B] = [C, 0, X] };

      let m = V - C;
      [R, G, B] = [R + m, G + m, B + m]

      R = Math.floor(R * 255)
      G = Math.floor(G * 255)
      B = Math.floor(B * 255)

      this.rgb.r = R
      this.rgb.g = G
      this.rgb.b = B
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #SvAreaSelectPoint{
    width: 12px;
    height: 12px;
    border-radius: 12px;
    border:1px solid #000;
    position: relative;
  }
  #SvArea{
    cursor: crosshair;
    width: 300px;
    height: 300px;
    background-image: linear-gradient(to bottom, transparent, #000000 ), linear-gradient(to right, #FFFFFF, transparent);
  }
  #HArea{
    width: 100px;
    height: 300px;
    background-image: linear-gradient( to top, rgb(255,0,0), rgb(255,255,0), rgb(0,255,0), rgb(0,255,255), rgb(0,0,255), rgb(255,0,255), rgb(255,0,0) );
  }
</style>
