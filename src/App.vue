<template>
  <div id="app">
    <todo-main></todo-main>
  </div>
</template>

<script>
import TodoMain from './components/TodoMain'

export default {
  name: 'App',
  components: {
    'todo-main': TodoMain
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
