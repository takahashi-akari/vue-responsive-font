<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "vueResponsiveFont",

  data() {
    return {
      fontSize: this.default,
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
    }
  },

  mounted() {
    this.responsiveFont();
    window.addEventListener("resize", this.responsiveFont);
  },

  watch: {
    min: function () {
      this.responsiveFont();
    },
    max: function () {
      this.responsiveFont();
    },
  },

  methods: {
    responsiveFont() {
      const wrapper = this.$refs.wrapper as Element;
      const wrapperWidth = wrapper.clientWidth;
      const wrapperHeight = wrapper.clientHeight;
      const wrapperRatio = wrapperWidth / wrapperHeight;

      const min = this.min;
      const max = this.max;
      const ratio = max / min;

      const fontSize = wrapperRatio > ratio ? max : wrapperWidth / ratio;

      this.fontSize = fontSize;
    }
  }
});
</script>

<template>
  <div ref="wrapper" class="vue-responsive-font" :style="{ fontSize: `${fontSize}px` }">
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
