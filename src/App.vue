<template>
  <div class="flex flex-col min-h-screen font-Roboto bg-weather-primary">
    <SiteNavigation />
    <li>hallo1</li>
    <li>hallo2</li>
    <li>hallo3</li>
    <li>hallo4</li>
    
    <Router-view />
  </div>
</template>

<script setup>
import { RouterView } from 'vue-router'
import SiteNavigation from './components/SiteNavigation.vue';
</script>

<style lang="scss" scoped>

</style>