<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "vueResponsiveFont",

  data() {
    return {
      fontSize: this.default as number,
      fontMin: this.min as number,
      fontMax: this.max as number,
    };
  },

  props: {
    min: {
      type: Number,
      default: 12,
    },
    max: {
      type: Number,
      default: 24,
    },
    default: {
      type: Number,
      default: 16,
    },
  },

  watch: {
    min: function (val) {
      this.fontMin = val;
    },
    max: function (val) {
      this.fontMax = val;
    },
    default: function (val) {
      this.fontSize = val;
    },
  },

  mounted() {
    const wrapper = this.$refs.wrapper as HTMLElement;
    wrapper.addEventListener("resize", () => {
      const wrapperWidth = wrapper.offsetWidth;
      const wrapperHeight = wrapper.offsetHeight;
      const wrapperRatio = wrapperWidth / wrapperHeight;
      const wrapperFontSize = wrapper.style.fontSize;
      const wrapperFontSizeNumber = Number(wrapperFontSize.replace("px", ""));
      const wrapperFontSizeRatio = wrapperFontSizeNumber / wrapperHeight;

      if (wrapperRatio > wrapperFontSizeRatio) {
        this.fontSize = wrapperHeight * wrapperFontSizeRatio;
      } else {
        this.fontSize = wrapperWidth / wrapperRatio;
      }

      if (this.fontSize < this.fontMin) {
        this.fontSize = this.fontMin;
      } else if (this.fontSize > this.fontMax) {
        this.fontSize = this.fontMax;
      }
    });
  },
});
</script>

<template>
  <div
    ref="wrapper"
    class="vue-responsive-font"
    :style="{ fontSize: fontSize + 'px' }"
  >
    <slot></slot>
  </div>
</template>

<style scoped>
.vue-responsive-font {
  display: inline-block;
  width: 100%;
  height: 100%;
  vertical-align: middle;
  line-height: 1;
}
</style>
