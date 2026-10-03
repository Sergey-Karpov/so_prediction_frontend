<template>
  <div class="prediction-form">
    <h2>Sales Prediction Form</h2>

    <form @submit.prevent="submitForm" class="prediction-form__form">
      <div class="prediction-form__wrapper">
        <!-- Chain -->
        <div class="prediction-form__field" :class="{ error: errors.chain }">
          <label for="chain">Retail chain *</label>
          <select id="chain" v-model="formData.chain" @blur="validateField('chain')">
            <option value="">Select a chain</option>
            <option value="aushan">Auchan</option>
            <option value="detmir">Detsky Mir</option>
            <option value="lenta">Lenta</option>
          </select>
          <span v-if="errors.chain" class="error-message">{{ errors.chain }}</span>
        </div>

        <!-- Cereals -->
        <div class="prediction-form__field" :class="{ error: errors.cereals }">
          <label for="cereals">Number of cereal SKUs *</label>
          <input
            id="cereals"
            type="number"
            v-model.number="formData.cereals"
            @blur="validateField('cereals')"
            placeholder="Cereal SKUs"
          />
          <span v-if="errors.cereals" class="error-message">{{ errors.cereals }}</span>
        </div>

        <!-- Milk -->
        <div class="prediction-form__field" :class="{ error: errors.milk }">
          <label for="milk">Number of milk SKUs *</label>
          <input
            id="milk"
            type="number"
            v-model.number="formData.milk"
            @blur="validateField('milk')"
            placeholder="Milk SKUs"
          />
          <span v-if="errors.milk" class="error-message">{{ errors.milk }}</span>
        </div>

        <!-- Population -->
        <div class="prediction-form__field" :class="{ error: errors.population }">
          <label for="population">City population *</label>
          <input
            id="population"
            type="number"
            v-model.number="formData.population"
            @blur="validateField('population')"
            placeholder="Enter the city population"
          />
          <span v-if="errors.population" class="error-message">{{ errors.population }}</span>
        </div>

        <!-- Market Share -->
        <div class="prediction-form__field" :class="{ error: errors.market_share }">
          <label for="market_share">Market share * (less than 1)</label>
          <input
            id="market_share"
            type="number"
            step="0.01"
            v-model.number="formData.market_share"
            @blur="validateField('market_share')"
            placeholder="0.00 - 0.99"
          />
          <span v-if="errors.market_share" class="error-message">{{ errors.market_share }}</span>
          <small class="hint">The value must be between 0 and 1 (e.g., 0.25)</small>
        </div>

        <!-- Aushan count -->
        <div class="prediction-form__field" :class="{ error: errors.aushan_count_in_city }">
          <label for="aushan">Number of Auchan stores in the city *</label>
          <input
            id="aushan"
            type="number"
            v-model.number="formData.aushan_count_in_city"
            @blur="validateField('aushan_count_in_city')"
            placeholder="Number of Auchan stores"
          />
          <span v-if="errors.aushan_count_in_city" class="error-message">{{
            errors.aushan_count_in_city
          }}</span>
        </div>

        <!-- Detmir count -->
        <div class="prediction-form__field" :class="{ error: errors.detmir_count_in_city }">
          <label for="detmir">Number of Detsky Mir stores in the city *</label>
          <input
            id="detmir"
            type="number"
            v-model.number="formData.detmir_count_in_city"
            @blur="validateField('detmir_count_in_city')"
            placeholder="Number of Detsky Mir stores"
          />
          <span v-if="errors.detmir_count_in_city" class="error-message">{{
            errors.detmir_count_in_city
          }}</span>
        </div>

        <!-- Lenta count -->
        <div class="prediction-form__field" :class="{ error: errors.lenta_count_in_city }">
          <label for="lenta">Number of Lenta stores in the city *</label>
          <input
            id="lenta"
            type="number"
            v-model.number="formData.lenta_count_in_city"
            @blur="validateField('lenta_count_in_city')"
            placeholder="Number of Lenta stores"
          />
          <span v-if="errors.lenta_count_in_city" class="error-message">{{
            errors.lenta_count_in_city
          }}</span>
        </div>
      </div>

      <button type="submit" class="submit-btn" :disabled="!isFormValid || isLoading">
        <span v-if="isLoading" class="spinner"></span>
        {{ isLoading ? 'Submitting...' : 'Predict' }}
      </button>
    </form>

    <!-- result -->
    <div v-if="predictionResult" class="prediction-form__result">
      <h3>Prediction Result</h3>
      <div class="prediction-form__result-value">
        <strong>{{ predictionResult }} ₽</strong>
      </div>
    </div>

    <!-- error -->
    <div v-if="serverError" class="server-error">
      {{ serverError }}
    </div>
  </div>
</template>

<script setup>
import { reactive, ref, computed } from 'vue'

const API_URL = import.meta.env.VITE_API_URL

// Form data
const formData = reactive({
  chain: '',
  cereals: null,
  milk: null,
  population: null,
  market_share: null,
  aushan_count_in_city: null,
  detmir_count_in_city: null,
  lenta_count_in_city: null,
})

// States
const isLoading = ref(false)
const predictionResult = ref(null)
const serverError = ref('')

// Validation errors
const errors = reactive({
  chain: '',
  cereals: '',
  milk: '',
  population: '',
  market_share: '',
  aushan_count_in_city: '',
  detmir_count_in_city: '',
  lenta_count_in_city: '',
})

// Field validation
const validateField = (field) => {
  switch (field) {
    case 'chain':
      if (!formData.chain) {
        errors.chain = 'Please select a retail chain'
      } else {
        errors.chain = ''
      }
      break

    case 'cereals':
      if (!formData.cereals && formData.cereals !== 0) {
        errors.cereals = 'Please enter the number of cereal SKUs'
      } else if (formData.cereals < 0) {
        errors.cereals = 'The value cannot be negative'
      } else {
        errors.cereals = ''
      }
      break

    case 'milk':
      if (!formData.milk && formData.milk !== 0) {
        errors.milk = 'Please enter the number of milk SKUs'
      } else if (formData.milk < 0) {
        errors.milk = 'The value cannot be negative'
      } else {
        errors.milk = ''
      }
      break

    case 'population':
      if (!formData.population && formData.population !== 0) {
        errors.population = 'Please enter the city population'
      } else if (formData.population < 0) {
        errors.population = 'Population cannot be negative'
      } else {
        errors.population = ''
      }
      break

    case 'market_share':
      if (!formData.market_share && formData.market_share !== 0) {
        errors.market_share = 'Please enter the market share'
      } else if (formData.market_share < 0) {
        errors.market_share = 'Market share cannot be negative'
      } else if (formData.market_share >= 1) {
        errors.market_share = 'Market share must be less than 1'
      } else {
        errors.market_share = ''
      }
      break

    case 'aushan_count_in_city':
      if (!formData.aushan_count_in_city && formData.aushan_count_in_city !== 0) {
        errors.aushan_count_in_city = 'Please enter the number of Auchan stores'
      } else if (formData.aushan_count_in_city < 0) {
        errors.aushan_count_in_city = 'The value cannot be negative'
      } else {
        errors.aushan_count_in_city = ''
      }
      break

    case 'detmir_count_in_city':
      if (!formData.detmir_count_in_city && formData.detmir_count_in_city !== 0) {
        errors.detmir_count_in_city = 'Please enter the number of Detsky Mir stores'
      } else if (formData.detmir_count_in_city < 0) {
        errors.detmir_count_in_city = 'The value cannot be negative'
      } else {
        errors.detmir_count_in_city = ''
      }
      break

    case 'lenta_count_in_city':
      if (!formData.lenta_count_in_city && formData.lenta_count_in_city !== 0) {
        errors.lenta_count_in_city = 'Please enter the number of Lenta stores'
      } else if (formData.lenta_count_in_city < 0) {
        errors.lenta_count_in_city = 'The value cannot be negative'
      } else {
        errors.lenta_count_in_city = ''
      }
      break
  }
}

// Full form validation
const isFormValid = computed(() => {
  // Ensure all fields are filled
  const allFieldsFilled =
    formData.chain &&
    formData.cereals !== null &&
    formData.milk !== null &&
    formData.population !== null &&
    formData.market_share !== null &&
    formData.aushan_count_in_city !== null &&
    formData.detmir_count_in_city !== null &&
    formData.lenta_count_in_city !== null

  // Ensure there are no validation errors
  const noErrors = Object.values(errors).every((error) => error === '')

  // Additional check: market_share < 1
  const marketShareValid = formData.market_share < 1

  return allFieldsFilled && noErrors && marketShareValid
})

// Form submission
const submitForm = async () => {
  // Validate all fields before submission
  Object.keys(formData).forEach((field) => validateField(field))

  if (!isFormValid.value) return

  isLoading.value = true
  serverError.value = ''
  predictionResult.value = null

  try {
    const response = await fetch(`${API_URL}predict`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(formData),
    })

    if (response.ok) {
      const result = await response.json()
      predictionResult.value = result.prediction

      console.log(predictionResult.value)

      // Optionally reset the form
      // Object.keys(formData).forEach(key => {
      //   formData[key] = key === 'chain' ? '' : null
      // })
    } else {
      const error = await response.json()
      serverError.value = error.error || 'An error occurred while submitting the data'
    }
  } catch (error) {
    console.error('Error:', error)
    serverError.value = 'The server is unavailable. Please try again later.'
  } finally {
    isLoading.value = false
  }
}
</script>

<style scoped lang="scss">
.prediction-form {
  min-width: 100%;
  padding: 2rem;
  border-radius: 12px;

  @media (max-width: 1024px) {
    padding: 1rem;
  }

  @media (max-width: 768px) {
    padding: 0.25rem;
  }

  h2 {
    text-align: center;
    color: hsla(160, 100%, 37%, 1);
    margin-bottom: 2rem;

    @media (max-width: 1024px) {
      margin-bottom: 1rem;
    }

    @media (max-width: 768px) {
      margin-bottom: 0.5rem;
    }
  }

  &__form {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    gap: 2rem;

    @media (max-width: 768px) {
      gap: 1rem;
    }
  }

  &__wrapper {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;

    @media (max-width: 1024px) {
      grid-template-columns: repeat(2, 1fr);
    }

    @media (max-width: 768px) {
      grid-template-columns: repeat(1, 1fr);
    }
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

    input,
    select {
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
      input,
      select {
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

    .hint {
      color: #999;
      font-size: 12px;
    }
  }

  .submit-btn {
    align-self: flex-end;
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

  &__result {
    margin-top: 30px;
    padding: 20px;
    background-color: #e8f5e9;
    border-radius: 8px;
    text-align: center;

    h3 {
      color: #2e7d32;
      margin-bottom: 15px;
    }

    &-value {
      font-size: 20px;

      strong {
        color: #42b883;
        font-size: 28px;
        margin-left: 10px;
      }
    }
  }

  .server-error {
    margin-top: 20px;
    padding: 15px;
    background-color: #fee;
    color: #c33;
    border-radius: 8px;
    text-align: center;
  }
}
</style>
