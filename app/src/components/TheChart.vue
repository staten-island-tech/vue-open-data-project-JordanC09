<template>
  <Pie :id="'my-chart-id'" :options="chartOptions" :data="chartData" />
</template>

<script setup>
import { Pie } from 'vue-chartjs'
import { defineProps } from 'vue'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  ArcElement,
  BarElement,
  CategoryScale,
  LinearScale,
} from 'chart.js'
import { onMounted, reactive } from 'vue'

ChartJS.register(Title, Tooltip, Legend, ArcElement, CategoryScale, LinearScale)
const props = defineProps({
  
  chartData: Object,
  chartOptions: Object,
})


// defineProps({
//   BarChart: Bar,
//   //pass in the animal which is an object
// })



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

<style scoped></style>
