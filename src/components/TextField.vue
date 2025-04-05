<template>
  <div class="text-field">
    <input 
      type="text"
      :placeholder="placeholder"
      v-model="inputValue"
      @keyup.enter="handleSubmit"
      :style="{
        borderRadius: `${borderRadius}px`
      }"
    >
    <button 
      class="submit-button"
      @click="handleSubmit"
      :style="{
        backgroundColor: primaryColor,
        borderRadius: `${borderRadius}px`
      }"
    >
      ↑
    </button>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'TextField',
  
  props: {
    placeholder: {
      type: String,
      default: 'Ask a question'
    },
    primaryColor: {
      type: String,
      default: '#007AFF'
    },
    borderRadius: {
      type: Number,
      default: 8
    }
  },

  setup() {
    const inputValue = ref('')

    const handleSubmit = () => {
      if (!inputValue.value.trim()) return
      
      // Handle submit
      console.log('Submitted:', inputValue.value)
      inputValue.value = ''
    }

    return {
      inputValue,
      handleSubmit
    }
  }
}
</script>

<style scoped>
.text-field {
  position: relative;
  display: flex;
  gap: 0.5rem;
}

input {
  flex: 1;
  padding: 0.75rem 1rem;
  border: 1px solid #E5E7EB;
  font-size: 0.875rem;
  transition: border-color 0.2s;
}

input:focus {
  outline: none;
  border-color: #9CA3AF;
}

.submit-button {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  border: none;
  color: white;
  font-size: 1.25rem;
  cursor: pointer;
  transition: opacity 0.2s;
}

.submit-button:hover {
  opacity: 0.9;
}
</style> 