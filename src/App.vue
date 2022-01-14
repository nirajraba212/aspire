<template>
  <div id="app">
    <div class="menu"><menu-bar/></div>
    <div class="container"><container></container></div>
  </div>
</template>

<script>
import menuBar from "./widgets/menu-bar.vue"
import container from "./widgets/container.vue"

export default {
  name: 'App',
  components: {
    menuBar,
    container
  }
}
</script>

<style>
</style>
