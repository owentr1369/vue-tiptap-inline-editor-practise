<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <TiptapEditor msg="Welcome to Your Vue.js App" />
</template>

<script>
import TiptapEditor from "./components/TiptapEditor.vue";

export default {
  name: "App",
  components: {
    TiptapEditor,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
