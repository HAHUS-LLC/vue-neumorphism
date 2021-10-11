<template>
  <button @click="click" class="style" :style="cssProps">
    <div class="hover">
      <slot></slot>
    </div>
    <div class="active">
      <slot></slot>
    </div>
    <slot></slot>
  </button>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import {hexToRgb, rgbToHex, colorLuminance} from "../../utils/hex-rgb-converter";

export default {
  name: "Button",
  methods: {
    click: function (e) {
      this.$emit('click', e);
    },
    darkerGradient: function () {
      console.log(this.shape !== 1 ? colorLuminance(this.color, 0.07) : this.color)
      console.log(this.shape !== 1 ? colorLuminance(this.color, -0.1) : this.color)
      return this.shape !== 1 ? colorLuminance(this.color, -0.1) : this.color
    },
    brightGradient: function () {
      return this.shape !== 1 ? colorLuminance(this.color, 0.17) : this.color
    },
    secondDarkerGradient: function () {
      return this.shape !== 1 ? colorLuminance(this.color, -0.2) : this.color
    },
    secondBrightGradient: function () {
      return this.shape !== 1 ? colorLuminance(this.color, 0.37) : this.color
    },
    darkerShadow: function () {
      return colorLuminance(this.backgroundColor, this.intensity * -1)
    },
    brightShadow: function () {
      return colorLuminance(this.backgroundColor, this.intensity)
    },
    innerDarkerShadow: function () {
      return colorLuminance(this.color, this.intensity * -1)
    },
    innerBrightShadow: function () {
      return colorLuminance(this.color, this.intensity)
    }
  },
  props: {
    'color': String,
    'backgroundColor': String,
    'deg': Number,
    'distance': Number,
    'blur': {
      type: Number,
      default: 1
    },
    'shape': {
      type: Number,
      default: 2,
    },
    'intensity': {
      type: Number,
      default: 0.15,
    }
  },
  computed: {
    cssProps() {
      return {
        '--baseColor': this.color,
        '--defaultBrightGradient': this.brightGradient(),
        '--defaultDarkerGradient': this.darkerGradient(),
        '--secondBrightGradient': this.secondBrightGradient(),
        '--secondDarkerGradient': this.secondDarkerGradient(),
        '--darkerShadow': this.darkerShadow(),
        '--brightShadow': this.brightShadow(),
        '--innerDarkerShadow': this.innerDarkerShadow(),
        '--innerBrightShadow': this.innerBrightShadow()
      }
    }
  }
}
</script>

<style scoped>
button {
  line-height: 1;
  text-align: center;
  -moz-user-select: -moz-none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  user-select: none;
  position: relative;
}

div {
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  border-radius: inherit;
  position: absolute;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  line-height: 1;
  text-align: center;
}

div span {
}

.float-shadow {
  box-shadow: 0px -10px 20px #FFFFFF, 0px 10px 40px #B8C2CC;
}

.default-shadow {
  box-shadow: -10px -10px 20px #FFFFFF, 10px 10px 20px #DAE0E6;
}

.style {
  background: linear-gradient(105deg, var(--defaultBrightGradient), var(--defaultDarkerGradient)) padding-box,
  linear-gradient(105deg, var(--secondBrightGradient), var(--secondDarkerGradient)) border-box;
  opacity: 1;
  border: 2px solid transparent;
  box-shadow: -10px -10px 20px var(--brightShadow), 10px 10px 20px var(--darkerShadow);
}

.hover, .active {
  opacity: 0;
  top: -2px;
  left: -2px;
  width: calc(100% + 0px);
  height: calc(100% + 0px);
  border: 2px solid transparent;
}

.hover {
  background: linear-gradient(105deg, var(--baseColor) 0%, var(--baseColor) 100%) padding-box,
  linear-gradient(105deg, var(--defaultBrightGradient), var(--defaultDarkerGradient)) border-box;
  box-shadow: 0px -10px 20px var(--brightShadow), 0px 10px 40px var(--darkerShadow);
  transition: ease .4s;
}

.active {
  background: linear-gradient(105deg, var(--defaultBrightGradient), var(--defaultDarkerGradient)) padding-box,
  linear-gradient(105deg, var(--secondBrightGradient), var(--secondDarkerGradient)) border-box;
  box-shadow: inset -4px -4px 8px 0px var(--innerBrightShadow), inset 4px 4px 8px 0px var(--innerDarkerShadow);
  transition: ease .2s;
}

.style:hover > .hover {
  opacity: 1;
}

.style:hover {
  box-shadow: transparent;
}

.style:active {
  box-shadow: -10px -10px 20px var(--brightShadow), 10px 10px 20px var(--darkerShadow);
}

.style:active > .hover {
  opacity: 0;
  transition: ease .2s !important;
}

.style:active > .active {
  opacity: 1;
}

span {
  color: white;
  font-weight: bold;
}
</style>
