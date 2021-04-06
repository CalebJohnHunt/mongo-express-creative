<template>
  <div id="app">
    <img src='/vue-creative-logo.svg' />
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/create">Create</router-link> |
      <router-link to="/palette">Palette</router-link> |
      <router-link to="/select-palette">Select Palette</router-link> |
      <router-link to="/edit/:paletteID/:swatchID">Edit</router-link>
    </div>
    <router-view/>
    <Footer />
  </div>
</template>

<script>
import Footer from './components/Footer.vue'

export default {
  components: {
    Footer,
  }
}
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* color: #2c3e50; */
}

img {
  max-width: 30px;
}

#nav {
  padding: 30px;
  padding-top: 10px;
}

#nav a {
  font-weight: bold;
  /* color: #2c3e50; */
  color: #1099B9;
}

#nav a.router-link-exact-active {
  padding: 10px;
  border-radius: 10px;
  background-color: #1099B9;
  color: white;
}

button {
  color: white;
  background-color: #1099B9;
}
</style>
