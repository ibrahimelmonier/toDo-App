<template>
  <div class="container" id="app">
    <img class="logo" alt="Vue logo" src="./assets/logo.png">
    <ToDoList/>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import ToDoList from './components/toDoList.vue'

export default {
  name: 'app',
  components: {
    // HelloWorld,
    ToDoList,
  }
}
</script>

<style>

  * {
    box-sizing: border-box;
  }

  .container {
    max-width: 600px;
    margin: 0 auto;
  }

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
  font-size: 20px;
}

  .logo {
    display: block;
    margin: 20px auto;
    height: 75px;
  }
</style>
