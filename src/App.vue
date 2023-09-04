 <script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>

    <div class=" w-auto flex flex-row justify-center bg-blue-900 p-4">
      <nav class="w-6/12 flex flex-row space-x-10">
        <RouterLink to="/" class="text-sm hover:text-blue-300 text-white">Inicio</RouterLink>
        <RouterLink to="/graficos-rt" class="text-sm hover:text-blue-300 text-white">Gráficos Real Time</RouterLink>
        <RouterLink to="/graficos-ht" class="text-sm hover:text-blue-300 text-white">Gráficos Histórico</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView class=" w-auto flex flex-row justify-center "/>
</template>

