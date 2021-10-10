<template>
  <div class="box">
    <div style="width: 80vw">
      <j-vue-inline-magnifier
        :src="img"
        style="max-height: 100%; cursor: grab"
        width="200px"
        height="140px"
        :zoom-factor="1.5"
      />
    </div>
  </div>
</template>

<script>
import JVueInlineMagnifier from "./components/JVueInlineMagnifier";

export default {
  name: "App",
  components: { JVueInlineMagnifier },
  data() {
    return {
      img:
        "https://i.etsystatic.com/6448977/r/il/6da6eb/1038951011/il_1588xN.1038951011_gajr.jpg",
    };
  },
};
</script>

<style lang="scss">
.box {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: #E2EEFC;
  margin: 0;
}
</style>
