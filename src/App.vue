<script setup>
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
</script>

<template>
  <Header />
  <RouterView />
  <Footer />
</template>

<style>
body {
  margin: 0;
  font-family: Questrial, sans-serif;
  font-size: 16px;
}
</style>
