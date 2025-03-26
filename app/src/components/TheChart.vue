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
import { onMounted, reactive } from 'vue'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)
defineProps({
  BarChart: Bar,
  //pass in the animal which is an object
})

const chartData = reactive({
  labels: ['Bronx', 'Kings', 'Brooklyn', 'Queens', 'Staten Island'],
  datasets: [{ data: [0, 0, 0, 0, 0] }],
})
const chartOptions = {
  responsive: true,
}

onMounted(async () => {
  try {
    const response = await fetch('https://data.cityofnewyork.us/resource/bqiq-cu78.json?$limit=500')
    if (response.status != 200) {
      throw new Error(response)
    } else {
      const data = await response.json()
      console.log(data)
      data.forEach((crime) => {
        // if (chartData.labels.includes(crime.county)) {
        //   console.log('bruh')
        // } else {
        //   chartData.labels.push(crime.county)
        //   console.log(chartData)
        console.log(0)
        if (crime.county === 'BRONX') {
          chartData.datasets[0].data[0] += 1
        }
        if (crime.county === 'KINGS') {
          chartData.datasets[0].data[1] += 1
        }
        if (crime.county === 'NEW YORK') {
          chartData.datasets[0].data[2] += 1
        }
        if (crime.county === 'QUEENS') {
          chartData.datasets[0].data[3] += 1
        }
        if (crime.county === 'RICHMOND') {
          chartData.datasets[0].data[4] += 1
        }
      })
    }
    console.log(chartData.datasets[0].data[0],chartData.datasets[0].data[1], chartData.datasets[0].data[2], chartData.datasets[0].data[3], chartData.datasets[0].data[4])
    chartData.datasets[0].data = [...chartData.datasets[0].data]
    
  } catch (error) {
    console.error('Error fetching data:', error)
    alert('hey I could not find that agent unc')
    
  }
})

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

<style lang="scss"></style>
