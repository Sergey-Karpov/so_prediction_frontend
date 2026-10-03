<template>
  <div class="login-view">
    <div class="login-view__card">
      <h2>Sign In</h2>
      <p class="login-view__subtitle">Enter your credentials to access the prediction service</p>

      <form @submit.prevent="submitForm" class="login-view__form">
        <!-- Username -->
        <div class="login-view__field" :class="{ error: errors.username }">
          <label for="username">Username *</label>
          <input
            id="username"
            type="text"
            v-model="formData.username"
            @blur="validateField('username')"
            placeholder="Enter your username"
            autocomplete="username"
          />
          <span v-if="errors.username" class="error-message">{{ errors.username }}</span>
        </div>

        <!-- Password -->
        <div class="login-view__field" :class="{ error: errors.password }">
          <label for="password">Password *</label>
          <input
            id="password"
            type="password"
            v-model="formData.password"
            @blur="validateField('password')"
            placeholder="Enter your password"
            autocomplete="current-password"
          />
          <span v-if="errors.password" class="error-message">{{ errors.password }}</span>
        </div>

        <!-- Auth error -->
        <div v-if="authError" class="login-view__auth-error">
          {{ authError }}
        </div>

        <button type="submit" class="submit-btn" :disabled="!isFormValid || isLoading">
          <span v-if="isLoading" class="spinner"></span>
          {{ isLoading ? 'Signing in...' : 'Sign In' }}
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref, computed } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

// ⚠️ Hardcoded credentials
const VALID_USERNAME = 'admin'
const VALID_PASSWORD = 'nutricia2026'

const formData = reactive({
  username: '',
  password: '',
})

const isLoading = ref(false)
const authError = ref('')

const errors = reactive({
  username: '',
  password: '',
})

const validateField = (field) => {
  switch (field) {
    case 'username':
      if (!formData.username) {
        errors.username = 'Please enter your username'
      } else {
        errors.username = ''
      }
      break

    case 'password':
      if (!formData.password) {
        errors.password = 'Please enter your password'
      } else if (formData.password.length < 4) {
        errors.password = 'Password must be at least 4 characters'
      } else {
        errors.password = ''
      }
      break
  }
}

const isFormValid = computed(() => {
  const allFieldsFilled = formData.username && formData.password
  const noErrors = Object.values(errors).every((error) => error === '')
  return allFieldsFilled && noErrors
})

const submitForm = async () => {
  Object.keys(formData).forEach((field) => validateField(field))
  if (!isFormValid.value) return

  isLoading.value = true
  authError.value = ''

  // Имитация задержки, как будто идёт запрос на сервер
  await new Promise((resolve) => setTimeout(resolve, 300))

  if (formData.username === VALID_USERNAME && formData.password === VALID_PASSWORD) {
    // Сохраняем флаг авторизации в sessionStorage
    sessionStorage.setItem('isAuthenticated', 'true')
    router.push('/home')
  } else {
    authError.value = 'Invalid username or password'
    formData.password = ''
  }

  isLoading.value = false
}
</script>

<style scoped lang="scss">
.login-view {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 60vh;
  padding: 2rem;

  @media (max-width: 768px) {
    padding: 1rem;
  }

  &__card {
    width: 100%;
    max-width: 420px;
    padding: 2.5rem;
    background: #fff;
    border-radius: 12px;
    box-shadow: 0 4px 24px rgba(0, 0, 0, 0.08);

    @media (max-width: 768px) {
      padding: 1.5rem;
    }
  }

  h2 {
    text-align: center;
    color: hsla(160, 100%, 37%, 1);
    margin-bottom: 0.5rem;
  }

  &__subtitle {
    text-align: center;
    color: #888;
    font-size: 13px;
    margin-bottom: 2rem;
  }

  &__form {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }

  &__field {
    display: flex;
    flex-direction: column;
    gap: 8px;
    position: relative;

    label {
      font-weight: 600;
      color: #555;
      font-size: 12px;
    }

    input {
      padding: 12px;
      border: 2px solid #e0e0e0;
      border-radius: 8px;
      font-size: 16px;
      transition: all 0.3s;

      &:focus {
        outline: none;
        border-color: #42b883;
      }
    }

    &.error {
      input {
        border-color: #f56c6c;
      }
    }

    .error-message {
      color: #f56c6c;
      font-size: 10px;
      position: absolute;
      top: calc(100% + 0.1rem);
      left: 0.25rem;
    }
  }

  &__auth-error {
    padding: 10px 12px;
    background-color: #fee;
    color: #c33;
    border-radius: 8px;
    font-size: 13px;
    text-align: center;
  }

  .submit-btn {
    padding: 14px;
    background-color: #42b883;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;

    &:hover:not(:disabled) {
      background-color: #33a06f;
      transform: translateY(-2px);
    }

    &:disabled {
      background-color: #ccc;
      cursor: not-allowed;
    }
  }

  .spinner {
    width: 20px;
    height: 20px;
    border: 2px solid white;
    border-top-color: transparent;
    border-radius: 50%;
    animation: spin 0.6s linear infinite;
  }

  @keyframes spin {
    to {
      transform: rotate(360deg);
    }
  }
}
</style>
