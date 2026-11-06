<script setup>
import { RouterView } from 'vue-router'
import Navigation from './components/Navigation.vue';
</script>

<template>
  <Navigation></Navigation>
  <RouterView />
</template>

<style>
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body {
  width: 100%;
  overflow-x: hidden;
  background-color: #faf8f5;
  color: #1a1918;
  font-family: 'Montserrat', sans-serif;
  -webkit-text-size-adjust: 100%;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  -webkit-tap-highlight-color: transparent;
}

/* Ensure images never overflow their container */
img {
  max-width: 100%;
  height: auto;
}
</style>
