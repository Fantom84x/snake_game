<template>
  <v-app>
    <v-main style="width: 100vw; height: 100vh; overflow-y: auto">
      <AppSnakeBoard style="overflow-y: auto"></AppSnakeBoard>
    </v-main>
  </v-app>
</template>

<script>
import AppSnakeBoard from './components/AppSnakeBoard.vue'

export default {
  components: {
    AppSnakeBoard,
  },
}
</script>

<style>
html, body {
  overflow-y: hidden;
}
</style>