<template>
  <div class="box">
    <div style="width: 300px">
      <j-vue-inline-magnifier
        :src="img"
        style="max-height: 100%"
        width="50px"
        height="50px"
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
        "https://images.unsplash.com/photo-1542856204-00101eb6def4?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=975&q=80",
    };
  },
};
</script>

<style lang="scss">
.box {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
