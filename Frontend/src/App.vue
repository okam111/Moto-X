<script setup>
import { RouterView } from 'vue-router'
import Navigation from './components/Navigation.vue';
</script>

<template>
  <Navigation></Navigation>
  <RouterView />
</template>

<style>
*{
  margin: 0;
  padding: 0;
}
</style>
