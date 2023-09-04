<template>
  <div>
    <Bar :options="chartOptions" :data="chartData" />
  </div>
  <h1>{{sensor1}}</h1>


</template>

<script setup>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
import {onMounted, reactive, ref} from "vue";
import axios from 'axios'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

const data = ref({})
const interval = ref(0)
const sensor1 = ref(0)
onMounted(
    interval.value = setInterval(async()=>{
        try {
          let dado = await axios.get('http://127.0.0.1:1880/sensores')
          data.value = dado.data
          console.log(data.value.sensor0)
          sensor1.value = data.value.sensor0
          console.log(sensor1.value)
          chartData.value.datasets[0].data[0] = parseFloat(data.value.sensor0)
          chartData.value.datasets[0].data[1] = parseFloat(data.value.sensor1)
          chartData.value.datasets[0].data[2] = parseFloat(data.value.sensor2)
        }catch (error){
          console.log(error)
        }
    },1000)

)


const chartData = ref({
  labels: [ 'January', 'February', 'March'],
  datasets: [
    {
      label: 'Data One',
      backgroundColor: '#f87979',
      data: [10, 20, 12]
    },
    {
      label: 'Data One',
      backgroundColor: '#000000',
      data: [40, 20, 12]
    }
  ]
})


const chartOptions =  reactive(
    {
      responsive: true
    }
)
</script>