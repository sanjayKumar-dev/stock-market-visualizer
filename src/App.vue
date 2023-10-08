<template>
  <v-card>
    <v-layout>
      <!-- <Headers /> -->
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
          <TradingViewChart @createdChart="setChart($event)"/>
        </div>
      </v-main>
    </v-layout>
  </v-card>
</template>

<script>
// import Headers from './components/GenricComponent/Header.vue'
import Sidenav from './components/GenricComponent/Sidenav.vue'
import TradingViewChart from './components/Chart/TradingViewChart.vue'

export default {
  name: 'App',
  components: {
    // Headers,
    Sidenav,
    TradingViewChart
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
    setChart(chart){
      console.log('set Chart', chart);
      this.candlestickSeries = chart
    },
    getDimensions() {
      // this.candlestickSeries.applyOptions({ height: window.innerWidth, width: window.innerHeight })
      console.log('getDimensions');
      // this.$refs.TradingViewChart.resizeChart()
    }
  }

}
</script>