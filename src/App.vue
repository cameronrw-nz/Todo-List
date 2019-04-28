<template>
  <div id="app">
    <TodoList msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import TodoList from "./components/TodoList.vue";

export default {
  name: "app",
  components: {
    TodoList
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #444444;
  height: 100%;
  display: flex;
  justify-content: center;
  align-content: center;
}

html,
body {
  height: 100%;
  padding: 0px;
  margin: 0px;
}
</style>
