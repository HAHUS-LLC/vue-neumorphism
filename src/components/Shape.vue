<template>
  <div @click="click" @focus="focus" @mouseover="hover" @mousedown="active" :style="cssProps">
    <Shape class="style">
      <slot></slot>
    </Shape>
    <Shape class="hover">
      <slot></slot>
    </Shape>
    <div></div>
    <Shape class="active">
      <slot></slot>
    </Shape>
    <Shape class="focus">
      <slot></slot>
    </Shape>
    <slot></slot>
  </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import {hexToRgb, rgbToHex, colorLuminance} from "../../utils/color-utils";

export default {
  name: "Base",
  methods: {
    click: function (e) {
      this.$emit('click', e);
    },
    focus: function (e) {
      this.$emit('focus', e);
    },
    hover: function (e) {
      this.$emit('hover', e);
    },
    active: function (e) {
      this.$emit('active', e);
    },
    darkerGradient: function () {
      // console.log(this.shape !== 1 ? colorLuminance(this.color, 0.07) : this.color)
      // console.log(this.shape !== 1 ? colorLuminance(this.color, -0.1) : this.color)
      return colorLuminance(this.color, -0.1)
    },
    brightGradient: function () {
      return colorLuminance(this.color, 0.17)
    },
    secondDarkerGradient: function () {
      return colorLuminance(this.color, -0.2)
    },
    secondBrightGradient: function () {
      return colorLuminance(this.color, 0.37)
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
    'intensity': {
      type: Number,
      default: 0.15,
    },
    'isHover': {
      type: Boolean,
      default: false,
    },
    'isClick': {
      type: Boolean,
      default: false,
    },
    'isFoucus': {
      type: Boolean,
      default: false,
    },
    'isActive': {
      type: Boolean,
      default: false,
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
div {
  line-height: 1;
  text-align: center;
  -moz-user-select: -moz-none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  user-select: none;
  position: relative;
}

.hover, .active, .focus, .style {
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
