<template>
  <div id="app" class="container">
    <TodoList/>
  </div>
</template>

<script>
import TodoList from './components/TodoList';

export default {
  name: 'App',
  components: {
    TodoList,
  },
}
</script>

<style>
.container{
    padding: 0 20px;
    margin: 0 auto;
    max-width: 100%;
}
#app {
  font-family: 'Noto Sans HK', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
@media screen and (max-width: 500px) {
  #app{
      margin-top: 30px;
  }
}
</style>
