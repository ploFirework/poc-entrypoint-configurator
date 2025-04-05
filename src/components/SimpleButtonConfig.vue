<template>
  <div class="simple-button-config">
    <div class="form-group">
      <label>Button Text</label>
      <input 
        type="text" 
        v-model="config.text"
        @input="updateConfig"
      >
    </div>

    <label class="checkbox-label">
      <input 
        type="checkbox" 
        v-model="config.showAiIcon"
        @change="updateConfig"
      > Show AI Icon
    </label>
  </div>
</template>

<script>
import { reactive, watch } from 'vue'

export default {
  name: 'SimpleButtonConfig',
  
  props: {
    modelValue: {
      type: Object,
      default: () => ({
        text: 'Click me',
        showAiIcon: true
      })
    }
  },

  emits: ['update:modelValue'],

  setup(props, { emit }) {
    const config = reactive({
      text: props.modelValue.text,
      showAiIcon: props.modelValue.showAiIcon
    })

    watch(() => props.modelValue, (newValue) => {
      config.text = newValue.text
      config.showAiIcon = newValue.showAiIcon
    })

    const updateConfig = () => {
      emit('update:modelValue', {
        text: config.text,
        showAiIcon: config.showAiIcon
      })
    }

    return {
      config,
      updateConfig
    }
  }
}
</script>

<style scoped>
.simple-button-config {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-group label {
  font-size: 0.875rem;
  font-weight: 500;
  color: #374151;
}

input[type="text"] {
  padding: 0.5rem;
  border: 1px solid #E5E7EB;
  border-radius: 4px;
  font-size: 0.875rem;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  font-weight: 500;
  color: #374151;
}
</style> 