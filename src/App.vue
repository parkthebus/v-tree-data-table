<template>
  <div id="app">
    <Grid/>
  </div>
</template>

<script>
import Vue from 'vue'
import Vuetify from 'vuetify'
import 'vuetify/dist/vuetify.min.css'

Vue.use(Vuetify, {
})
import Grid from "./components/Grid";

export default {
  name: "App",
  components: {
    Grid
  }
};
</script>