<template>
  <Bar id="my-chart-id" :options="chartOptions" :data="chartData" />
</template>

<script setup>
import { Bar } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)
defineProps({
  chart: Object,
  //pass in the animal which is an object
})
let chartData = {
  labels: ['January', 'February', 'March'],
  datasets: [{ data: [40, 20, 12] }],
}
let chartOptions = {
  responsive: true,
}
async function dadata() {
  try {
    const response = await fetch('https://data.cityofnewyork.us/resource/bqiq-cu78.json')
    if (response.status != 200) {
      throw new Error(response)
    } else {
      let data = await response.json()
      console.log(data)
      data.forEach((crime) => {
        if (chartData.labels.includes(crime.county)) {
          console.log('bruh')
        } else {
          chartData.push(crime.county)
          console.log(chartData)
        }
      })
    }
  } catch (error) {
    alert('hey I could not find that agent unc')
  }
}
dadata()
// export default {
//   name: 'BarChart',
//   components: { Bar },
//   data() {
//     return {
//       chartData: {
//         labels: ['January', 'February', 'March'],
//         datasets: [{ data: [40, 20, 12] }],
//       },
//       chartOptions: {
//         responsive: true,
//       },
//     }
//   },
// }
</script>

<style lang="scss" scoped></style>
