<template>
  <div class="text-field-config">
    <div class="form-group">
      <label>Placeholder Text</label>
      <input 
        type="text" 
        v-model="config.placeholder"
        @input="updateConfig"
      >
    </div>
  </div>
</template>

<script>
import { reactive, watch } from 'vue'

export default {
  name: 'TextFieldConfig',
  
  props: {
    modelValue: {
      type: Object,
      default: () => ({
        placeholder: 'Ask a question'
      })
    }
  },

  emits: ['update:modelValue'],

  setup(props, { emit }) {
    const config = reactive({
      placeholder: props.modelValue.placeholder
    })

    watch(() => props.modelValue, (newValue) => {
      config.placeholder = newValue.placeholder
    })

    const updateConfig = () => {
      emit('update:modelValue', {
        placeholder: config.placeholder
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
.text-field-config {
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
</style> 