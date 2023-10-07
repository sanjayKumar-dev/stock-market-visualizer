<template>
  <v-app>
    <v-main>
      <Headers />
    </v-main>
  </v-app>
</template>

<script>
import Headers from './components/Header.vue'

export default {
  name: 'App',

  components: {
    Headers
  },
}
</script>