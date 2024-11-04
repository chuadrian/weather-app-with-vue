<template>
  <div class="app">
    <weather-app></weather-app>
  </div>
</template>

<script>
import WeatherApp from './components/WeatherApp.vue'

export default {
  name: 'App',
  components: {
    WeatherApp
  }
}
</script>

<style>
.app {
  width: 100%;
  min-height: 100vh;
  padding: 20px;
}
</style>