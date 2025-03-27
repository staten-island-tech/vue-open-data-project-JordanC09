<template>
  <div>
    <h1 class="text-3xl font-bold underline">Hate Crimes by category in Kings</h1>
    <RouterLink to="/"
      ><button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full">
        Back to Home
      </button></RouterLink
    >
    <RouterLink to="/about"
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
      <TheChart :chartData="chartData" :chartOptions="chartOptions" />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import TheChart from '@/components/TheChart.vue'

//let things = []
// Religion/Religious Practice"
// 1
// :
// "Sexual Orientation"
// 2
// :
// "Race/Color"
// 3
// :
// "Gender"
// 4
// :
// "Ethnicity/National Origin/Ancestry"

const chartData = ref({
  labels: ['Religion', 'Sexual Orientation', 'Race', 'Gender', 'Ancestry'],
  datasets: [
    {
      data: [0, 0, 0, 0, 0],
      backgroundColor: ['#FF5733', '#33FF57', '#3357FF', '#FF33A1', '#FFB833'],
      hoverOffset: 4,
    },
  ],
})

const chartOptions = ref({
  responsive: true,
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
        if (crime.county === 'KINGS') {
          if (crime.offense_category === 'Religion/Religious Practice') {
            crimeCounts[0] += 1
          }
          if (crime.offense_category === 'Sexual Orientation') {
            crimeCounts[1] += 1
          }
          if (crime.offense_category === 'Race/Color') {
            crimeCounts[2] += 1
          }
          if (crime.offense_category === 'Gender') {
            crimeCounts[3] += 1
          }
          if (crime.offense_category === 'Ethnicity/National Origin/Ancestry') {
            crimeCounts[4] += 1
          }
          //if (things.includes(crime.offense_category)){
          //console.log("bruh")
          //}else{
          //things.push(crime.offense_category)
          //}
        }
      })
      //console.log(things)

      chartData.value = {
        labels: ['Religion', 'Sexual Orientation', 'Race', 'Gender', 'Ancestry'],
        datasets: [
          {
            data: crimeCounts,
          },
        ],
      }
    }
  } catch (error) {
    console.error('Error fetching data:', error)
  }
})
</script>
