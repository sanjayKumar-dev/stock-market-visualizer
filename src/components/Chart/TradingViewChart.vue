<template>
    <div>
        <v-form v-model="valid">
            <v-container>
                <v-row>
                    <v-col cols="8" md="4">
                        <v-autocomplete label="Stock Symbles" density="compact" v-model="filterForm.stock"
                            :items="['California', 'Colorado', 'Florida', 'Georgia', 'Texas', 'Wyoming']"></v-autocomplete>
                    </v-col>

                    <v-col cols="8" md="2">
                        <v-select density="compact" v-model="filterForm.timeFrame" label="Time Frame" :items="['1H', '1D']"></v-select>
                    </v-col>

                    <v-col cols="8" md="2">
                        <v-select density="compact" v-model="filterForm.toc" label="Type Of Chart" :items="['Line Chart', 'Candlestick Chart']"></v-select>
                    </v-col>

                    <v-col cols="8" md="4">
                        <v-btn size="large"  block v-on:click='submitFilterForm()'>
                            Button
                        </v-btn>
                    </v-col>
                </v-row>
            </v-container>
        </v-form>

    </div>
    <div ref="resizeDiv" class="d-flex resize-div">
        <div ref="chartContainer"></div>
    </div>
</template>
  
<script>
import { createChart } from 'lightweight-charts'
import { ChartDailyData } from '/src/MockData/chartdailyData.js'

export default {
    data() {
        return {
            candlestickSeries: null,
            filterForm: {
                stock: '',
                timeFrame: '',
                toc: ''
            }
        };
    },
    mounted() {
        console.log('Im text inside the component.');

        const chart = createChart(this.$refs.chartContainer, {
            width: this.chartWidth - 15,
            height: this.chartHeight,
        })
        this.candlestickSeries = chart.addCandlestickSeries()
        const data = ChartDailyData
        this.candlestickSeries.setData(data);
        this.emmitData()
        window.addEventListener('resize', this.updateChartDimensions())
    },
    methods: {
        updateChartDimensions() {
            console.log('Dimensions Chnage');
            const div = this.$refs.resizeDiv;
            const rect = div.getBoundingClientRect();
            this.divWidth = rect.width;
            this.divHeight = rect.height;
            // this.candlestickSeries.resize(this.chartWidth, this.chartHeight);
        },
        emmitData() {
            this.$emit('createdChart', this.candlestickSeries)
        },
        resizeChart() {
            console.log('resize');
            this.candlestickSeries.applyOptions({ height: window.innerWidth, width: window.innerHeight })
        },
        submitFilterForm(){
            console.log(this.filterForm)
        }
        
    },
    unmounted() {
        window.removeEventListener('resize', this.updateChartDimensions);
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
  
<style scoped></style>
  