

<template>
  <!-- Lets make a simple image magnifier -->
  <div class="magnify" ref="container">
    <div
      style="position: relative; max-height: 100%; cursor: grab"
      ref="zoom"
      @mousemove="mouseMove"
      @mouseenter="mouseEnter"
      @mouseleave="mouseLeave"
      @touchstart="touchStart"
      @touchmove="touchMove"
      @touchend="touchEnd"
    >
      <!--img inherits the height from its parent-->

      <img
        ref="image"
        :src="src"
        width="100%"
        style="max-height: inherit; object-fit: cover"
        alt="vue-inline-zoom main image"
      />
      <transition name="fade">
        <div
          v-if="showZoom"
          ref="glass"
          :style="{
            background: 'url(' + src + ') no-repeat #fff',
            border: '4px solid #efefef',
            zIndex: 99,
            display: 'block',
            width: `${glassWidth}px`,
            height: `${glassHeight}px`,
            boxShadow: '0 5px 10px -2px rgba(0, 0, 0, 0.3)',
            pointerEvents: 'none',
            position: 'absolute',
          }"
        />
      </transition>
    </div>
  </div>
</template>

<script>
/**
 * When the user hovers on the image,
 * the script will first calculate
 * the native dimensions if they don't exist.
 * Only after the native dimensions are available,
 * the script will show the zoomed version.
 */
export default {
  name: "vue-inline-zoom",
  props: {
    src: {
      type: String,
      required: true,
    },
    zoomFactor: {
      type: Number,
      default: 1.5,
    },
  },
  data() {
    return {
      glassWidth: 0,
      glassHeight: 0,
      showZoom: true,
    };
  },
  methods: {
    showMagnifier() {
      this.showZoom = true;
      // and define dimensions for the magnifying glass
      // to be 30% of the actual image
      this.glassWidth = this.$refs.image.width * 0.3;
      this.glassHeight = this.$refs.image.height * 0.3;
    },
    moveMagnifier(event) {
      const rect = event.target.getBoundingClientRect();
      const x = event.clientX - rect.left; //x position within the element.
      const y = event.clientY - rect.top; //y position within the element.
      //const x = event.pageX - this.$refs.image.parentElement.offset().left;
      // const y =event.pageY - this.$refs.image.parentElement.offset().top;
      const magnified = this.$refs.glass,
        style = magnified.style,
        imgWidth = this.$refs.image.width,
        imgHeight = this.$refs.image.height;
      let xperc = (x / imgWidth) * 100;
      let yperc = (y / imgHeight) * 100;

      // Add some margin for right edge
      if (x > 0.01 * imgWidth) {
        xperc += 0.15 * xperc;
      }

      // Add some margin for bottom edge
      if (y >= 0.01 * imgHeight) {
        yperc += 0.15 * yperc;
      }

      // Set the background of the magnified image horizontal
      style.backgroundPositionX = xperc - 20 + "%";
      // Set the background of the magnified image vertical
      style.backgroundPositionY = yperc - 20 + "%";
      // Move the magnifying glass with the mouse movement.
      style.left = x - 50 + "px";
      style.top = y - 50 + "px";

      style.transform = `scale(${this.zoomFactor})`;
    },
    async mouseEnter() {
      this.showMagnifier();
    },
    mouseMove(event) {
      this.moveMagnifier(event);
    },
    mouseLeave() {
      this.showZoom = false;
    },

    touchStart(event) {
      if (event.type === "touchstart") {
        this.showMagnifier();
      }
    },
    touchEnd(event) {
      this.showZoom = false;
    },
    touchMove(event) {
      if (this.showZoom) {
        if (event.type === "touchmove") {
          for (let touch of event.touches) {
            this.moveMagnifier(touch);
          }
        }
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
