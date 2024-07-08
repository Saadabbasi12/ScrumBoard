<template>
  <div id="app">
    <ScrumBoard />
  </div>
</template>

<script>
import ScrumBoard from './components/ScrumBoard.vue'

export default {
  name: 'App',
  components: {
    ScrumBoard
  }
}
</script>

<style>
@import 'assets/main.css';
</style>
