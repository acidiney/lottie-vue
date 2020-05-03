<template>
  <div id="app" style="height: 100vh; width: 100vw">
    <lottie :animation="Lost" :speed="4" loop auto-play/>
  </div>
</template>

<script>
import Lottie from "./Lottie.vue";

export default {
  name: "App",
  components: {
    Lottie
  },
  computed: {
    Lost() {
      return require("./losted-astro.json");
    }
  }
};
</script>