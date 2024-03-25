<template>
  <div id="app">
    <TestList />
  </div>
</template>

<script>
import TestList from './components/TestList.vue';
// import WindowEditor from "./components/WindowEditor.vue";

export default {
  name: "App",
  components: {
    // WindowEditor,
    TestList,
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
