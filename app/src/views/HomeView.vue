<template>
  <div>
    <h1 class="text-3xl font-bold underline">Hate Crimes across the 5 Boroughs</h1>
    <RouterLink to="/about"
      ><button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full">
        Bronx
      </button></RouterLink
    >
    <RouterLink to="/Kings"
      ><button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full">
        Brooklyn
      </button></RouterLink
    >
    <RouterLink to="/Brooklyn"
      ><button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full">
        Manhattan
      </button></RouterLink
    >
    <RouterLink to="/Queens"
      ><button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full">
        Queens
      </button></RouterLink
    >
    <RouterLink to="/SI"
      ><button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full">
        Staten Island
      </button></RouterLink
    >

    <div class="w-full max-w-md h-1000 mx-auto">
      <BarChart :chartData="chartData" :chartOptions="chartOptions" />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import BarChart from '@/components/BarChart.vue'

const chartData = ref({
  labels: ['Bronx', 'Kings', 'Brooklyn', 'Queens', 'Staten Island'],
  datasets: [
    {
      label: 'Hate Crime Data',
      data: [0, 0, 0, 0, 0],
      backgroundColor: ['#FF5733', '#33FF57', '#3357FF', '#FF33A1', '#FFB833'],

      borderColor: [
        'rgb(0, 0, 0)',
        'rgb(0, 0, 0)',
        'rgb(0, 0, 0)',
        'rgb(0, 0, 0)',
        'rgb(0, 0, 0)',
        'rgb(0, 0, 0)',
        'rgb(0, 0, 0)',
      ],
      borderWidth: 1,
    },
  ],
})

const chartOptions = ref({
  responsive: true,
  plugins: {
    legend: {
      position: 'top',
      labels: {
        color: '#333',
        font: {
          size: 14,
          weight: 'bold',
        },
      },
    },
    tooltip: {
      callbacks: {
        label: function (tooltipItem) {
          return tooltipItem.label + ': ' + tooltipItem.raw + ' crimes'
        },
      },
    },
  },
  scales: {
    y: {
      beginAtZero: true,
    },
  },
})

onMounted(async () => {
  try {
    const response = await fetch(
      'https://data.cityofnewyork.us/resource/bqiq-cu78.json?$limit=2000',
    )
    if (response.status !== 200) {
      throw new Error(response)
    } else {
      const data = await response.json()
      console.log(data)

      let crimeCounts = [0, 0, 0, 0, 0]

      data.forEach((crime) => {
        if (crime.county === 'BRONX') {
          crimeCounts[0] += 1
        }
        if (crime.county === 'KINGS') {
          crimeCounts[1] += 1
        }
        if (crime.county === 'NEW YORK') {
          crimeCounts[2] += 1
        }
        if (crime.county === 'QUEENS') {
          crimeCounts[3] += 1
        }
        if (crime.county === 'RICHMOND') {
          crimeCounts[4] += 1
        }
      })

      chartData.value = {
        labels: ['Bronx', 'Kings', 'Brooklyn', 'Queens', 'Staten Island'],
        datasets: [
          {
            data: crimeCounts,
            label: 'Hate Crime Data',

            backgroundColor: ['#FF5733', '#33FF57', '#3357FF', '#FF33A1', '#FFB833'],

            borderColor: [
              'rgb(0, 0, 0)',
              'rgb(0, 0, 0)',
              'rgb(0, 0, 0)',
              'rgb(0, 0, 0)',
              'rgb(0, 0, 0)',
              'rgb(0, 0, 0)',
              'rgb(0, 0, 0)',
            ],
            borderWidth: 1,
          },
        ],
      }
    }
  } catch (error) {
    console.error('Error fetching data:', error)
  }
})
</script>
