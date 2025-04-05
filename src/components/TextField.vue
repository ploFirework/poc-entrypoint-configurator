<template>
  <div 
    class="text-field"
    :style="{
      borderRadius: `${borderRadius}px`
    }"
  >
    <input 
      type="text"
      :placeholder="placeholder"
      v-model="inputValue"
      @keyup.enter="handleSubmit"
    >
    <button 
      class="submit-button"
      @click="handleSubmit"
      :style="{
        backgroundColor: primaryColor,
        borderRadius: `${borderRadius}px`
      }"
    >
      <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M8 3.33333L8 12.6667" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
        <path d="M3.33331 8L7.99998 3.33333L12.6666 8" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
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
  border: 1px solid #E5E7EB;
  transition: border-color 0.2s;
}

.text-field:focus-within {
  border-color: #9CA3AF;
}

input {
  flex: 1;
  padding: 0.75rem 1rem;
  padding-right: calc(0.75rem * 2 + 38px); /* Make room for the button */
  border: none;
  font-size: 0.875rem;
  background: transparent;
}

input:focus {
  outline: none;
}

.submit-button {
  position: absolute;
  right: 4px;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  align-items: center;
  justify-content: center;
  aspect-ratio: 1;
  height: calc(100% - 8px);
  border: none;
  color: white;
  cursor: pointer;
  transition: opacity 0.2s;
}

.submit-button:hover {
  opacity: 0.9;
}
</style> 