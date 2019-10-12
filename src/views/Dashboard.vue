<template>
  <div>
<div class="conatiner">
<div class="row">
<div class="col-md-4">  <canvas id="chart1" class="w-100"></canvas></div>
<div class="col-md-4"><canvas id="chart2" class="w-100"></canvas></div>
<div class="col-md-4"> <canvas id="chart3" class="w-100"></canvas></div>
</div>
</div>
  </div>
</template>

<script>
import Chart from 'chart.js'
import { Bar } from 'vue-chartjs'
import planetChartData from '@/chart-data.js'


export default {
  extends: Bar,
  mounted () {
  // Promise chain to draw charts as per priority (line, bar, pie)
    this.createChart('chart1', this.planetChartData, 'line')
      .then(result => {
        this.createChart('chart2', this.planetChartData, 'bar')
          .then(result => {
            this.createChart('chart3', this.planetChartData, 'pie')
          })
      })
      .catch(err => {
        console.log('error', err)
        throw err
      })
  },
  name: 'dashboard',
  data () {
    return {
      title: 'Dashboard',
      planetChartData: planetChartData
    }
  },
  methods: {
    createChart (chartId, chartData, chartType) {
      return new Promise(function (resolve, reject) {
      // callback if animation is not complete
        setTimeout(
          (function () {
            let isComplete
            chartData.options.animation = {
              onComplete: function () {
                isComplete = true
                resolve(isComplete, null)
              }
            }
            const ctx = document.getElementById(chartId)
            const drawChart = new Chart(ctx, {
              type: String(chartType),
              data: chartData.data,
              options: chartData.options
            })
          })(), 5)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  font-weight: normal;
}

.center-charts {
  display:inline-block
}
.w{
  width:100% !important;
}
</style>
