<template>
  <v-card>
    <v-layout>
      <v-app-bar color="warn" prominent>
        <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        <v-toolbar-title>My files</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn variant="text" icon="mdi-magnify"></v-btn>
        <v-btn variant="text" icon="mdi-filter"></v-btn>
        <v-btn variant="text" icon="mdi-dots-vertical"></v-btn>
      </v-app-bar>

      <v-navigation-drawer v-model="drawer" temporary>
        <Sidenav />
      </v-navigation-drawer>

      <v-main>
        <div>
          <TradingViewChart />
        </div>
      </v-main>
    </v-layout>
  </v-card>
</template>

<script>
import Sidenav from './components/GenricComponent/Sidenav.vue'
import TradingViewChart from './components/Chart/TradingViewChart.vue'

export default {
  name: 'App',
  components: {
    Sidenav,
    TradingViewChart,
  },

  data() {
    return {
      drawer: false,
      candlestickSeries: null
    }
  },
  mounted() {
    window.addEventListener('resize', this.getDimensions);
  },
  methods: {
    getDimensions() {
      console.log('getDimensions');
    }
  }

}
</script>