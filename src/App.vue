<script setup>
import { RouterView } from 'vue-router'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

//import './App.css';

</script>

<template>
  <div class="main">
    <Header></Header>
    
      <RouterView />
    <Footer></Footer>
  </div>
</template>

<style scoped>

</style>
