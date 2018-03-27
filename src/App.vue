<template>
  <div id="app">
    <HomePage/>
  </div>
</template>

<script>
import HomePage from './components/HomePage'

export default {
  name: 'App',
  components: {
    HomePage
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
}
</style>
