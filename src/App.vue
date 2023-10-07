<template>
  <v-app>
    <v-main>
      <Headers />
      <Sidenav />
    </v-main>
  </v-app>
</template>

<script>
import Headers from './components/GenricComponent/Header.vue'
import Sidenav from './components/GenricComponent/Sidenav.vue';

export default {
  name: 'App',

  components: {
    Headers,
    Sidenav
  },
}
</script>