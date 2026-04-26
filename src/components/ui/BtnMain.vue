<template>
  <button :disabled="isDisabled" @click="clickHandler" class="btn">
    <slot>
      {{ buttonTxt }}
    </slot>
  </button>
</template>

<script setup>
const props = defineProps({
  buttonTxt: {
    type: String,
    default: 'Start',
  },
  isDisabled: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits(['clickHandler'])

const clickHandler = () => {
  if (!props.isDisabled) {
    emit('clickHandler')
  }
}
</script>

<style scoped lang="scss">
.btn {
  padding: 15px 30px;
  font-size: 18px;
  background-color: hsla(160, 100%, 37%, 1);
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
  width: 30%;

  &:hover {
    background-color: #33a06f;
  }

  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    pointer-events: none;
  }

  @media (max-width: 1024px) {
    align-self: flex-start;
    width: 40%;
  }

  @media (max-width: 768px) {
    width: 100%;
  }
}
</style>
