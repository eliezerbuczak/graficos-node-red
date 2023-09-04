<template>
  <div>
    <canvas id="grafico" width="400" height="400"></canvas>
  </div>
  <h1 id="sensor_01"></h1>
</template>

<script setup>
import {onMounted, onUpdated, ref} from "vue";
import axios from 'axios'
import Chart from "chart.js/auto";

const valores = ref([])
const interval = ref(0)
const date = ref({})

const chartCanvas = ref(null)
let grafico = null

const mountedChart = async () => {

  chartCanvas.value = document.getElementById('grafico')

  let ctx = chartCanvas.value.getContext('2d')
  console.log(ctx)
  grafico = new Chart(ctx, {
    type: 'bar',
    data: {
      labels: [10,9,8,7,6,5,4,3,2,1],
      datasets: [{
        label: '# of Votes',
        data: valores.value,
        borderWidth: 1
      }]
    },
    options: {
      scales: {
        y: {
          beginAtZero: true
        }
      },
      responsive: true,
      maintainAspectRatio: false,
    }
  })

}
const getData = async () => {
  let historico = []
  interval.value = setInterval(async()=>{

  try {
    date.value = await axios.get('http://localhost:1880/sensores')
    date.value = date.value.data

    let sensor = [date.value.sensor0, date.value.date_at]
    historico.unshift(sensor)

    if(historico.length> 11){
      historico.pop()
    }
    let tam = 10
    for (let i = 0; i <= tam; i++) {
      valores.value[tam - i] = historico[i][1]
    }
    console.log(historico)
    grafico.update()
    document.getElementById('sensor_01').innerHTML = date.value.sensor1

  }catch (error){
    console.log(error)
  }
},1000)

}

onMounted(  ()=>{
  mountedChart()
  getData()

})
</script>