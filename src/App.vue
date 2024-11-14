<template>
  <div id="app">
    <TodoContainer />
  </div>
</template>

<script>

import TodoContainer from './components/todos/TodoContainer.vue';

export default {
  name: 'App',
  components: {
    TodoContainer
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
