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
        "https://slack-imgs.com/?c=1&o1=ro&url=https%3A%2F%2Fres.cloudinary.com%2Fpracticaldev%2Fimage%2Ffetch%2Fs--GaTHUcpn--%2Fc_imagga_scale%2Cf_auto%2Cfl_progressive%2Ch_500%2Cq_auto%2Cw_1000%2Fhttps%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fi%2F2ac98nfp3nxprfbfbk25.png",
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
