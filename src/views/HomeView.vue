<script setup>
import { ref } from 'vue'
import BtnMain from '../components/ui/BtnMain.vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const isLoading = ref(false)
const errorMsg = ref('')

const checkServer = async () => {
  isLoading.value = true
  errorMsg.value = ''

  try {
    const response = await fetch('https://sales-prediction-api.up.railway.app/health')
    if (response.ok) {
      router.push('/form')
      console.log('ok')
    } else {
      errorMsg.value = `Сервер вернул ошибку: ${response.status}`
    }
  } catch (error) {
    console.error('Ошибка запроса, ', error)
    errorMsg.value = 'Сервер недоступен, проверте подключение к интернету'
  } finally {
    isLoading.value = false
  }
}
</script>

<template>
  <main>
    <BtnMain :disabled="isLoading" @click-handler="checkServer"> Start </BtnMain>
  </main>
</template>
