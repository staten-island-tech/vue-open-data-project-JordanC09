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
  chart: Object,
  //pass in the animal which is an object
})
let Bronx = 0
let Kings = 0
let NewYork = 0
let Queens = 0
let Richmond = 0

let chartData = {
  labels: ['Bronx', 'Kings', 'Brroklyn', 'Queens', 'Staten Island'],
  datasets: [{ data: [Bronx, Kings, NewYork, Queens, Richmond] }],
}
let chartOptions = {
  responsive: true,
}

onMounted(async () => {
  try {
    const response = await fetch('https://data.cityofnewyork.us/resource/bqiq-cu78.json?$limit=100')
    if (response.status != 200) {
      throw new Error(response)
    } else {
      let data = await response.json()
      console.log(data)
      data.forEach((crime) => {
        // if (chartData.labels.includes(crime.county)) {
        //   console.log('bruh')
        // } else {
        //   chartData.labels.push(crime.county)
        //   console.log(chartData)
        console.log(crime.county)
        if (crime.county === 'BRONX') {
          Bronx += 1
        }
        if (crime.county === 'QUEENS') {
          chartData.datasets.data[1] += 1
        }
        if (crime.county === 'NEW YORK') {
          NewYork += 1
        }
        if (crime.county === 'KINGS') {
          Kings += 1
        }
        if (crime.county === 'RICHMOND') {
          Richmond += 1
        }
      })
    }
    console.log(chartData.labels)
    console.log(Bronx, Kings, NewYork, Queens, Richmond)
  } catch (error) {
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

<style lang="scss" scoped></style>
