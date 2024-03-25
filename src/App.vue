<template>
  <div id="app">
    <WindowEditor />
  </div>
</template>

<script>
import WindowEditor from "./components/WindowEditor.vue";

export default {
  name: "App",
  components: {
    WindowEditor,
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  background-color: #464646;
  display: flex;
  flex-direction: row;
  align-items: flex-start
}
</style>  
