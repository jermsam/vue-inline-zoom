<template>
  <!-- Lets make a simple image magnifier -->
  <div class="magnify" ref="container">
    <div
      style="postion: relative; max-height: 100%; cursor: 'grab'"
      ref="zoom"
      @mousemove="mouseMove"
      @mouseenter="mouseEnter"
      @mouseleave="mouseLeave"
    >
      <!--img inherits the height from its parent-->

      <img
        ref="image"
        :src="src"
        width="100%"
        style="max-height: inherit; object-fit: cover"
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
  name: "my-bad",
  props: {
    src: {
      type: String,
      required: true,
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
    async mouseEnter() {
      // fade in when mouse enters the image container
      this.showZoom = true;
      // and define dimensions for the magnifying glass
      // to be 30% of the actual image
      this.glassWidth = this.$refs.image.width * 0.3;
      this.glassHeight = this.$refs.image.height * 0.3;
    },
    mouseMove(event) {
      // event
      // x/y coordinates of the mouse
      // clientX/Y gives the coordinates relative to the viewport in CSS pixels.
      // console.log(event.clientX);
      // console.log(event.clientY);
      // pageX/Y gives the coordinates relative to the <html> element in CSS pixels.
      // console.log(event.pageX);
      // console.log(event.pageY);
      // screenX/Y gives the coordinates relative to the screen in device pixels.
      // console.log(event.screenX);
      // console.log(event.screenY);
      // const container = this.$refs.container;
      //  const magnify_offset = container.offset();

      //Mouse positions with respect to the container

      const rect = event.target.getBoundingClientRect();
      const x = event.clientX - rect.left; //x position within the element.
      const y = event.clientY - rect.top; //y position within the element.

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
      style.backgroundPositionX = xperc - 9 + "%";
      // Set the background of the magnified image vertical
      style.backgroundPositionY = yperc - 9 + "%";

      // Move the magnifying glass with the mouse movement.
      style.left = x - 50 + "px";
      style.top = y - 50 + "px";

      // // console.log(glass);
      // if (this.showZoom) {
      //   //The background position of .large will be changed according to the position
      //   //of the mouse over the .small image. So we will get the ratio of the pixel
      //   //under the mouse pointer with respect to the image and use that to position the
      //   //large image inside the magnifying glass
      //   const rx =
      //     Math.round((mx / image.width) * this.native_width - glass.width / 2) *
      //     -1;
      //   const ry =
      //     Math.round(
      //       (my / image.height) * this.native_height - glass.height / 2
      //     ) * -1;
      //   this.bgp = rx + "px " + ry + "px";
      //   //Time to move the magnifying glass with the mouse
      //   this.px = mx - glass.width / 2;
      //   this.py = my - glass.height / 2;
      //   //Now the glass moves with the mouse
      //   //The logic is to deduct half of the glass's width and height from the
      //   //mouse coordinates to place it with its center at the mouse coordinates
      //   //If you hover on the image now, you should see the magnifying glass in action
      // }
    },
    mouseLeave() {
      // fade out when mouse leaves the image
      this.showZoom = false;
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