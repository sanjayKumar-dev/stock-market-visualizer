<template>
    <div>
        <SnackBar :snackBarData="snackBarData" />
        <v-form v-model="valid">
            <v-container>
                <v-row>
                    <v-col cols="8" md="4">
                        <v-autocomplete label="Stock Symbles" density="compact" v-model="filterForm.stock" :items="stckList"
                            :item-title="'name'" :item-value="'symbol'"></v-autocomplete>
                    </v-col>

                    <v-col cols="8" md="2">
                        <v-select density="compact" v-model="filterForm.timeFrame" label="Time Frame"
                            :items="['1H', '1D']"></v-select>
                    </v-col>

                    <v-col cols="8" md="2">
                        <v-select density="compact" v-model="filterForm.toc" label="Type Of Chart"
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
    <div v-if="!isDataSelected" class="initialMgs">
        <h3>Please select Any stock to Start Analysing</h3>
    </div>
    <div class="d-flex resize-div">
        <div ref="chartContainer"></div>
    </div>
</template>
  
<script>
import { createChart } from 'lightweight-charts'
import { ChartDailyData } from '/src/MockData/chartdailyData.js'
import { StockList } from '/src/MockData/stockList.js'
import axios from 'axios'
import SnackBar from '/src/components/GenricComponent/Snackbar.vue'

export default {
    components: {
        SnackBar
    },
    data() {
        return {
            isDataSelected: false,
            stckList: StockList,
            candlestickSeries: null,
            filterForm: {
                stock: '',
                timeFrame: '',
                toc: ''
            },
            snackBarData: {
                mgs: '',
                type: '',
                showSnackBar: false
            }
        }
    },
    methods: {
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
            console.log(this.filterForm)
            this.snackBarMgs = 'Testing 123'
            this.getStockData()
        },
        async getStockData() {
            try {
                await axios.get(`${process.env.VUE_APP_STOCKS_URI}/api/search?symbol=NIFTYBANK&period=1D`).then(response => {
                    if (response.data.statusCode === 'SUCCESS') {
                        console.log('res', response.data)
                        this.snackBarData = {
                            mgs: response.data.message,
                            type: 'success',
                            showSnackBar: true
                        }
                        // this.isDataSelected = true
                        this.createChart()
                    } else {
                        console.log(response.data.statusCode)
                        this.snackBarData = {
                            mgs: response.data.message,
                            type: 'error',
                            showSnackBar: true
                        }
                    }
                })

            } catch (err) {
                this.snackBarData = {
                    mgs: 'Error while retrieveing the data',
                    type: 'error',
                    showSnackBar: true
                }
            }
        },
        createChart() {
            const chart = createChart(this.$refs.chartContainer, {
                width: this.chartWidth - 15,
                height: this.chartHeight - 200,
            })
            this.candlestickSeries = chart.addCandlestickSeries()
            const data = ChartDailyData
            this.candlestickSeries.setData(data)
            this.isDataSelected = true
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
};
</script>
  
<style scoped>
.initialMgs {
    font-style: italic;
}
</style>
  