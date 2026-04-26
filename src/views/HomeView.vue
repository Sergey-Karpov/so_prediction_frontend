<script setup>
import { onMounted, ref } from 'vue'
import BtnMain from '@/components/ui/BtnMain.vue'
import { useRouter } from 'vue-router'
import MainArticle from '../components/ui/MainArticle.vue'

const router = useRouter()
const isLoading = ref(false)
const errorMsg = ref('')
const API_URL = import.meta.env.VITE_API_URL

onMounted(() => {
  console.log(API_URL)
})

const checkServer = async () => {
  isLoading.value = true
  errorMsg.value = ''

  try {
    const response = await fetch(API_URL)
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
    <div class="description">
      <MainArticle />
    </div>
    <BtnMain :disabled="isLoading" @click-handler="checkServer"> Start </BtnMain>
  </main>
</template>
