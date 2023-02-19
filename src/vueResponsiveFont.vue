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
    const observer = new MutationObserver(() => {
      const width = wrapper.getBoundingClientRect().width;
      const height = wrapper.getBoundingClientRect().height;
      const fontSize = Math.min(
        Math.max(
          Math.floor((width / height) * this.fontMax),
          this.fontMin
        ),
        this.fontMax
      );
      this.fontSize = fontSize;
    });
    observer.observe(wrapper, {
      attributes: true,
      childList: true,
      characterData: true,
      subtree: true,
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
