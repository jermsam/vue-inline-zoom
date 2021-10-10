<template>
  <div
    class="container"
    v-bind="$attrs"
    v-on="$listeners"
    @mouseenter="process_start"
    touchstart="process_start"
    @mousemove="process_move"
    @touchmove="process_move"
    @mouseleave="process_end"
    @touchend="process_end"
  >
    <div
      v-if="showGlass"
      class="magnifying-glass"
      ref="magnifier"
      :style="{
        background: `url(${src}) no-repeat`,
        width,
        height,
        borderRadius,
      }"
    />
    <img
      alt="Vue logo"
      :src="src"
      class="image"
      ref="image"
      width="100%"
      style="max-height: inherit; object-fit: cover"
    />
  </div>
</template>

<script>
export default {
  name: "j-vue-inline-magnifier",
  props: {
    src: {
      type: String,
      required: true,
    },
    width: {
      type: String,
      default: "100px",
    },
    height: {
      type: String,
      default: "100px",
    },
    borderRadius: {
      type: String,
      default: "1%",
    },
    zoomFactor: {
      type: Number,
      default: 1,
    },
  },
  data() {
    return {
      showGlass: false,
    };
  },
  methods: {
    process_start(e) {
      this.showGlass = e.type !== "click";
    },
    process_move(e) {
      if (this.showGlass) {
        //reference image and magnifying glass
        const img = this.$refs.image;
        const glass = this.$refs.magnifier;
        // get magnifying glass dimensions
        const bw = 1; // border width for glass
        // get the magnifying glass center position in reference to glass
        const cx = glass.offsetWidth / 2;
        const cy = glass.offsetHeight / 2;
        /*get the x and y positions of the image:*/
        const imgRect = img.getBoundingClientRect();
        // X posn at which the mouse was clicked - X posn of image top-left edge
        let x = e.pageX - imgRect.x;
        // Y posn at which the mouse was clicked - Y posn of image top-left edge
        let y = e.pageY - imgRect.y;

        /*consider any page scrolling:*/
        x = x - window.pageXOffset; // scroll in X direction
        y = y - window.pageYOffset; // scroll in Y direction
        const zoom = this.zoomFactor;
        /*prevent the magnifier glass from being positioned outside the image:*/
        if (x > img.width - cx / zoom) {
          x = img.width - cx / zoom;
        }
        if (x < cx / zoom) {
          x = cx / zoom;
        }
        if (y > img.height - cy / zoom) {
          y = img.height - cy / zoom;
        }
        if (y < cy / zoom) {
          y = cy / zoom;
        }
        /* Set the position of the magnifier glass: */
        glass.style.left = `${x - cx}px`;
        glass.style.top = `${y - cy}px`;

        /*display what the magnifier glass "sees":*/
        glass.style.backgroundSize = `${img.width * zoom}px ${
          img.height * zoom
        }px`;
        glass.style.backgroundPosition = `-${x * zoom - cx + bw}px -${
          y * zoom - cy + bw
        }px`;
        // console.log(glass.style.backgroundPosition);
      }
    },
    process_end() {
      this.showGlass = false;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.container {
  background: transparent;
  max-width: 100%;
  height: auto;
  padding: 0;
  box-sizing: border-box;
  position: relative;
  .magnifying-glass {
    border: 1px solid #aaa;
    z-index: 99;
    display: block;
    box-shadow: 0 5px 10px -2px rgba(0, 0, 0, 0.3);
    pointer-events: none;
    position: absolute;
    cursor: grab;
    box-sizing: border-box;
  }
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.5s ease;
  }

  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }
}
</style>
