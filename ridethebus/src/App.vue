<template>
  <div id="app">
    <Header msg="Welcome to RideTheBus!"/>
    <Game />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Game from './components/Game.vue'

export default {
  name: 'App',
  components: {
    Header,
    Game
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #22303f;
  margin-top: 60px;
}
</style>
