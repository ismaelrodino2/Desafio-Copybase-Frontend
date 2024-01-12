<template>
  <div class="min-h-screen flex flex-col items-center justify-center">
    <main class="text-4xl font-bold mb-4">Home</main>
    <form @submit.prevent="handleSubmit" class="flex items-center gap-4 pt-8">
      <input
        id="fileInput"
        type="file"
        ref="fileInputRef"
        class="hidden"
        @change="updateFileName"
      />
      <label for="fileInput" class="bg-blue-500 text-white py-2 px-4 rounded-md cursor-pointer">
        {{ fileName || 'Escolher arquivo' }}
      </label>
      <button type="submit" class="bg-green-500 text-white py-2 px-4 rounded-md">Enviar</button>
    </form>

    <div v-if="seusDadosMRR.length > 0 || seusDadosChurnRate.length > 0" class="pt-4">
      <MrrChurnCharts :mrrData="seusDadosMRR" :churnRateData="seusDadosChurnRate" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import MrrChurnCharts from '../components/MrrChurnCharts.vue'

const fileInputRef = ref<HTMLInputElement | null>(null)
const fileName = ref('')

const seusDadosMRR = ref([])
const seusDadosChurnRate = ref([])

const updateFileName = () => {
  if (fileInputRef.value?.files?.[0]) {
    fileName.value = fileInputRef.value.files[0].name
  }
}

const handleSubmit = async (event: Event) => {
  event.preventDefault()

  const fileInput = fileInputRef.value
  if (!fileInput || !fileInput.files) {
    return
  }

  const file = fileInput.files[0]
  console.log('file', file)

  if (file) {
    const formDataToSubmit = new FormData()
    formDataToSubmit.append('excelFile', file)

    try {
      const response = await fetch('http://localhost:3001/excel', {
        method: 'POST',
        body: formDataToSubmit
      })
      const data = await response.json()
      console.log('response', response, 'data', data)

      seusDadosMRR.value = data.mrr
      seusDadosChurnRate.value = data.churnRate

      console.log('seusDadosMRR', seusDadosMRR.value)
    } catch (error) {
      console.error('Error during file upload:', error)
    }
  }
}
</script>
