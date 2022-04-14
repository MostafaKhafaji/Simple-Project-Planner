<template>
  <NavBar />
  <router-view />
</template>
<script>
import NavBar from "./components/Navbar.vue";
export default {
  components: { NavBar },
};
</script>

<style>
body {
  background-color: #f2f2f2;
}
#app {
  font-family: "Grape Nuts", cursive;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 600px;
  margin: 0 auto;
  color: #555;
}
</style>
