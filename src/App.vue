<template>
  <div id="app">
    <MobileVideoRecorder />
  </div>
</template>

<script setup>
import MobileVideoRecorder from './components/MobileVideoRecorder.vue';
</script>

<style>
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  min-height: 100vh;
  background: #f4f4f4;
}
</style>
