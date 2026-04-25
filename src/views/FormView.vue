<script setup>
import { onMounted, ref } from 'vue'
import PredictionForm from '@/components/ui/PredictionForm.vue'

const modelDocs = ref(null)
const errorMsg = ref('')
const isLoading = ref(false)
const API_URL = import.meta.env.VITE_API_URL

onMounted(async () => {
  isLoading.value = true
  errorMsg.value = ''

  try {
    const response = await fetch(API_URL)
    if (response.ok) {
      modelDocs.value = await response.json()
      console.log(modelDocs.value)
    } else {
      errorMsg.value = `Сервер вернул ошибку: ${response.status}`
    }
  } catch (error) {
    console.error('Ошибка запроса, ', error)
    errorMsg.value = 'Сервер недоступен, проверте подключение к интернету'
  } finally {
    isLoading.value = false
  }
})
</script>

<template>
  <div class="about">
    <div v-if="isLoading">...загрузка...</div>

    <div v-else-if="errorMsg">
      {{ errorMsg }}
    </div>

    <div v-else-if="modelDocs">
      <div class="about__description">
        <h3>{{ modelDocs.message }}</h3>
        <ul>
          <li>
            <span>version: </span>
            <p>{{ modelDocs.version }}</p>
          </li>
          <li>
            <span>model type: </span>
            <p>{{ modelDocs.type }}</p>
          </li>
          <li>
            <span>status: </span>
            <p>{{ modelDocs.status }}</p>
          </li>
        </ul>
      </div>

      <PredictionForm />
    </div>
  </div>
</template>

<style scoped lang="scss">
.about {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}
</style>
