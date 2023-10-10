<template>
    <div class="inputFeild">
        <SnackBar :snackBarData="snackBarData" />
        <v-form v-model="valid">
            <v-container>
                <v-row>
                    <v-col cols="8" md="4">
                        <v-autocomplete label="Stock Symbles" density="compact" v-model="filterForm.stock" :items="stckList"
                            :item-title="'name'" :item-value="'symbol'"
                            :rules="[v => !!v || 'Name is required']"></v-autocomplete>
                    </v-col>

                    <v-col cols="8" md="2">
                        <v-select density="compact" v-model="filterForm.timeFrame" label="Time Frame"
                            :rules="[v => !!v || 'Timeframe is required']" :items="['1H', '1D']"></v-select>
                    </v-col>

                    <v-col cols="8" md="2">
                        <v-select density="compact" v-model="typeOfChart" label="Type Of Chart"
                            :items="['Line Chart', 'Candlestick Chart']"></v-select>
                    </v-col>

                    <v-col cols="8" md="4">
                        <v-btn size="large" block v-on:click='submitFilterForm()'>
                            Button
                        </v-btn>
                    </v-col>
                </v-row>
            </v-container>
        </v-form>

    </div>
    <div v-if="isloading" class="text-center">
        <v-progress-circular :size="70" :width="7" color="purple" indeterminate></v-progress-circular>

    </div>
    <div v-if="!isDataSelected" class="initialMgs">
        <h3>Please select Any stock to Start Analysing</h3>
    </div>
    <div ref="chartContainer"></div>
</template>
  
<script>
import { createChart } from 'lightweight-charts'
import { StockList } from '/src/MockData/stockList.js'
import axios from 'axios'
import SnackBar from '/src/components/GenricComponent/Snackbar.vue'

export default {
    components: {
        SnackBar
    },
    data() {
        return {
            isloading: false,
            isDataSelected: false,
            stckList: StockList,
            tvchart: null,
            candlestickSeries: null,
            lineSeries: null,
            chartData: [],
            typeOfChart: 'Candlestick Chart',
            filterForm: {
                stock: 'NIFTY50',
                timeFrame: '1D'
            },
            snackBarData: {
                mgs: '',
                type: '',
                showSnackBar: false
            }
        }
    },
    watch: {
        typeOfChart() {
            this.setSpecificChart()
        }
    },
    methods: {
        convertOHLCdataToLine() {
            let lineData = []
            this.chartData.forEach((data) => {
                lineData.push({
                    time: data.time,
                    value: data.close
                })
            })
            return lineData
        },
        setSpecificChart() {
            if (this.isDataSelected) {
                if (this.typeOfChart === 'Line Chart') {
                    this.lineSeries = this.tvchart.addLineSeries()
                    this.lineSeries.setData(this.convertOHLCdataToLine())
                    this.candlestickSeries.applyOptions({
                        visible: false,
                    })
                    this.lineSeries.applyOptions({
                        visible: true
                    })
                } else {
                    this.candlestickSeries = this.tvchart.addCandlestickSeries()
                    this.candlestickSeries.setData(this.chartData)
                    if (this.lineSeries) {
                        this.lineSeries.applyOptions({
                            visible: false
                        })
                    }
                    this.candlestickSeries.applyOptions({
                        visible: true,
                    })
                }
            }

        },
        updateChartDimensions() {
            const div = this.$refs.resizeDiv;
            const rect = div.getBoundingClientRect();
            this.divWidth = rect.width;
            this.divHeight = rect.height;
        },
        resizeChart() {
            this.candlestickSeries.applyOptions({ height: window.innerWidth, width: window.innerHeight })
        },
        submitFilterForm() {
            this.isloading = true
            this.getStockData(this.filterForm.stock, this.filterForm.timeFrame)
        },
        async getStockData(symbol, timeFrame) {
            try {
                await axios.get(`${process.env.VUE_APP_STOCKS_URI}/api/search?symbol=${symbol}&period=${timeFrame}`).then(response => {
                    if (response.data.statusCode === 'SUCCESS') {
                        this.snackBarData = {
                            mgs: response.data.message,
                            type: 'success',
                            showSnackBar: true
                        }
                        this.chartData = response.data.result
                        if (this.isDataSelected) {
                            this.updateChartData(response.data)

                        } else {
                            this.createChart(response.data)
                        }
                        this.isloading = false

                    } else {
                        this.isloading = false
                        this.snackBarData = {
                            mgs: response.data.message,
                            type: 'error',
                            showSnackBar: true
                        }
                    }
                })
            } catch (err) {
                this.isloading = false
                this.snackBarData = {
                    mgs: 'Something went Wrong',
                    type: 'error',
                    showSnackBar: true
                }
            }
        },
        updateChartData() {
            if (this.typeOfChart === 'Line Chart') {
                this.lineSeries.setData(this.convertOHLCdataToLine())
            } else {
                this.candlestickSeries.setData(this.chartData)
            }
        },
        createChart() {
            this.tvchart = createChart(this.$refs.chartContainer, {
                width: this.chartWidth - 15,
                height: this.chartHeight - 200,
            })
            this.isDataSelected = true
            this.setSpecificChart()
        }
    },
    computed: {
        chartWidth() {
            return window.innerWidth;
        },
        chartHeight() {
            return window.innerHeight;
        },
    },
}
</script>
  
<style scoped>
.initialMgs {
    font-style: italic;
}
</style>
  