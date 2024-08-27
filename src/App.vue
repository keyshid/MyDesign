<template>
  <div id="app">
    <DrugCounter />
    <!-- <DrugImpactStats /> -->
  </div>
</template>

<script setup>
import DrugCounter from './components/DrugCounter.vue';
import DrugImpactStats from './components/DrugImpactStats.vue';
</script>

<style>
/* Global styles */
body,
html {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
}
</style>
