<template>
  <nav class="nav bg-secondary text-white" id="header">
    <router-link class="nav-link" to="/">Home</router-link>
    <router-link class="nav-link" to="/about">About</router-link>
  </nav>
  <router-view/>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#header > a {
  color: white !important;
}
</style>
