<script setup>
import NavBar from './components/NavBar.vue'
</script>

<template>
  <NavBar />

  <RouterView />
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

body {
  font-family: 'Poppins', sans-serif;
  margin: 0 auto;

  padding: 0;
  overflow-x: auto;
}
html,
body {
  overflow-x: hidden;
}
</style>
