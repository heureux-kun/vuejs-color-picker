<template>
  <div id="Wrapper">
    <div id="ColorPicker">
      <!-- 彩度と明度を決定するエリア -->
      <div id="SvArea" v-bind:style="setSvAreaBackgroundColor" @click="getSv" v-model="hsvToRgb,rgbToHex">
        <!-- カーソルの位置を意味する○ -->
        <div id="SvAreaSelectPoint" v-bind:style="svAreaCursorStyle"></div>
      </div>

      <!-- 色相を決定するエリア -->
      <div id="HArea" @click="getH" v-model="hsvToRgb,rgbToHex"></div>

      <!-- 結果を表示するエリア -->
      <div id="ResultArea">
        <!--
        hAreaCoordinateY:{{ hAreaCoordinateY }}<br />
        degree:{{ hsv.h }}<br />
        highBrightnessRgb: {{ highBrightnessRgb.r }},{{ highBrightnessRgb.g }},{{ highBrightnessRgb.b }}
        -->
        <!-- 選択した色を表示するエリア -->
        <div id="SelectColorArea" v-bind:style="selectColor" ></div>
        #{{ hexCode }}<br />
        H：{{hsv.h}}° S：{{hsv.s}}% V：{{hsv.v}}%<br />
        R：{{rgb.r}} G：{{rgb.g}} B：{{rgb.b}}
      </div>
    </div>
    <div id="GrayScale">
      <ul>
        <li :style="grayScaleStyle01">{{ grayScaleHex01 }}</li>
        <li :style="grayScaleStyle02"></li>
        <li :style="grayScaleStyle03"></li>
        <li :style="grayScaleStyle04"></li>
        <li :style="grayScaleStyle05"></li>
        <li :style="grayScaleStyle06"></li>
        <li :style="grayScaleStyle07"></li>
        <li :style="grayScaleStyle08"></li>
        <li :style="grayScaleStyle09"></li>
        <li :style="grayScaleStyle10"></li>
        <li :style="grayScaleStyle11"></li>
      </ul>
    </div>
    <!-- Target -->
    <input id="foo" value="https://github.com/zenorocha/clipboard.js.git">
    <!-- Trigger -->
    <button class="btn" data-clipboard-target="#foo">クリック</button>
  </div>
</template>

<script>
import clipboard from 'clipboard'

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
        r: '255',
        g: '0',
        b: '0'
      },
      hexCode: '',
      grayScale: {
        color01: 0,
        color02: 26,
        color03: 51,
        color04: 77,
        color05: 102,
        color06: 128,
        color07: 153,
        color08: 179,
        color09: 201,
        color10: 230,
        color11: 255
      }
    }
  },
  computed: {
    setSvAreaBackgroundColor: function () {
      return 'background-color:rgb(' + this.highBrightnessRgb.r + ',' + this.highBrightnessRgb.g + ',' + this.highBrightnessRgb.b + ')'
    },
    svAreaCursorStyle: function () {
      let x = this.svAreaCoordinateX - 6
      let y = this.svAreaCoordinateY - 6
      let color
      if (y < 150) {
        color = '#000'
      } else {
        color = '#FFF'
      }
      return 'left: ' + x + 'px;' + 'top: ' + y + 'px;' + 'border-color: ' + color
    },
    selectColor: function () {
      return 'background-color:rgb(' + this.rgb.r + ',' + this.rgb.g + ',' + this.rgb.b + ')'
    },
    hsvToRgb: function () {
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
    },
    rgbToHex: function () {
      // rgbToHex
      let rToHex = this.rgb.r.toString(16)
      if (rToHex.length === 1) {
        rToHex = '0' + rToHex
      }
      let gToHex = this.rgb.g.toString(16)
      if (gToHex.length === 1) {
        gToHex = '0' + gToHex
      }
      let bToHex = this.rgb.b.toString(16)
      if (bToHex.length === 1) {
        bToHex = '0' + bToHex
      }
      this.hexCode = (rToHex + gToHex + bToHex).toUpperCase()
    },
    grayScaleStyle01: function () {
      let color = this.grayScale.color01
      return 'background-color:rgb(' + color + ',' + color + ',' + color + ')'
    },
    grayScaleStyle02: function () {
      let color = this.grayScale.color02
      return 'background-color:rgb(' + color + ',' + color + ',' + color + ')'
    },
    grayScaleStyle03: function () {
      let color = this.grayScale.color03
      return 'background-color:rgb(' + color + ',' + color + ',' + color + ')'
    },
    grayScaleStyle04: function () {
      let color = this.grayScale.color04
      return 'background-color:rgb(' + color + ',' + color + ',' + color + ')'
    },
    grayScaleStyle05: function () {
      let color = this.grayScale.color05
      return 'background-color:rgb(' + color + ',' + color + ',' + color + ')'
    },
    grayScaleStyle06: function () {
      let color = this.grayScale.color06
      return 'background-color:rgb(' + color + ',' + color + ',' + color + ')'
    },
    grayScaleStyle07: function () {
      let color = this.grayScale.color07
      return 'background-color:rgb(' + color + ',' + color + ',' + color + ')'
    },
    grayScaleStyle08: function () {
      let color = this.grayScale.color08
      return 'background-color:rgb(' + color + ',' + color + ',' + color + ')'
    },
    grayScaleStyle09: function () {
      let color = this.grayScale.color09
      return 'background-color:rgb(' + color + ',' + color + ',' + color + ')'
    },
    grayScaleStyle10: function () {
      let color = this.grayScale.color10
      return 'background-color:rgb(' + color + ',' + color + ',' + color + ')'
    },
    grayScaleStyle11: function () {
      let color = this.grayScale.color11
      return 'background-color:rgb(' + color + ',' + color + ',' + color + ')'
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
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #Wrapper{
    padding:20px;
    background-color: #000;
  }
  #ColorPicker{
    display: flex;
    color: #FFF;
  }
  #SvAreaSelectPoint{
    width: 12px;
    height: 12px;
    border-radius: 12px;
    border-style: solid;
    border-width: 1px;
    position: relative;
  }
  #SvArea{
    flex-basis: 300px;
    height: 300px;
    margin-right: 15px;
    cursor: crosshair;
    background-image: linear-gradient(to bottom, transparent, #000000 ), linear-gradient(to right, #FFFFFF, transparent);
  }
  #HArea{
    flex-basis: 50px;
    height: 300px;
    margin-right: 15px;
    background-image: linear-gradient( to top, rgb(255,0,0), rgb(255,255,0), rgb(0,255,0), rgb(0,255,255), rgb(0,0,255), rgb(255,0,255), rgb(255,0,0) );
  }
  #ResultArea{
    flex-basis: 200px;
  }
  #SelectColorArea{
    width: 100px;
    height: 100px;
    border:1px solid #000;
  }
  #GrayScale{
    margin-top: 20px;
  }
  #GrayScale ul{
    display: flex;
  }
  #GrayScale li{
    width: 30px;
    height: 30px;
  }
</style>
