<template>
  <v-ons-page id="app">
    <v-ons-toolbar class="home-toolbar">
      <div class="center">{{ title }}</div>
    </v-ons-toolbar>
    <home/>
  </v-ons-page>
</template>

<script>
import Home from './pages/Home'

export default {
  name: 'app',
  components: {
    Home
  },
  data () {
    return {
      title: 'Numbers Game'
    }
  }
}
</script>

<style scoped>
.center {
  text-align: center;
}
</style>
