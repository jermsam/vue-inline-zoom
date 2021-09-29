<template>
  <div id="app">
    <my-bad :src="img" />
  </div>
</template>

<script>
import MyBad from "./components/MyBad";

export default {
  name: "App",
  components: { MyBad },
  data() {
    return {
      img:
        "https://images.unsplash.com/photo-1542856204-00101eb6def4?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=975&q=80",
    };
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
