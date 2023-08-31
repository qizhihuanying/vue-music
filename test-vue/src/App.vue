<template>
  <div id="app">
    <h1>My Tone.js App</h1>
    <Recorder />
  </div>
</template>

<script>
import Recorder from './components/Recorder.vue'

export default {
  name: 'App',
  components: {
    Recorder
  }
}
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>