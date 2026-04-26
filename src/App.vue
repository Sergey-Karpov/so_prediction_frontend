<script setup>
import { RouterView } from 'vue-router'
import HeaderMain from '@/components/HeaderMain.vue'
</script>

<template>
  <div class="container">
    <HeaderMain />
    <RouterView />
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  gap: 2rem;
  padding-bottom: 4rem;
}
</style>
