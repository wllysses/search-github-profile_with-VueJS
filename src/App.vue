<template>
  <div id="app">
    <Search />
  </div>
</template>

<script>
import Search from './components/Search.vue'

export default {
  name: 'App',
  components: {
    Search
  }
}
</script>

<style>
#app {
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
</style>
