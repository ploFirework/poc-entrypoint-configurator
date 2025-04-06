<template>
  <div class="button-list-config">
    <div class="form-group">
      <label>Questions (one per line)</label>
      <textarea
        v-model="buttonsText"
        @change="updateConfig"
        rows="4"
        placeholder="Enter button text, one per line"
      ></textarea>
    </div>

    <div class="form-group">
      <label>Layout</label>
      <div class="layout-selector">
        <label class="layout-option">
          <input 
            type="radio" 
            v-model="config.layout" 
            value="horizontal"
            @change="updateConfig"
          >
          <span class="layout-label">Horizontal</span>
        </label>
        <label class="layout-option">
          <input 
            type="radio" 
            v-model="config.layout" 
            value="vertical"
            @change="updateConfig"
          >
          <span class="layout-label">Vertical</span>
        </label>
      </div>
    </div>

    <label class="checkbox-label">
      <input 
        type="checkbox" 
        v-model="config.showAskAnother"
        @change="updateConfig"
      > Show "Ask another question" Button
    </label>
  </div>
</template>

<script>
import { ref, reactive, watch } from 'vue'

export default {
  name: 'ButtonListConfig',
  
  props: {
    modelValue: {
      type: Object,
      default: () => ({
        buttons: [],
        layout: 'horizontal',
        showAskAnother: true
      })
    }
  },

  emits: ['update:modelValue'],

  setup(props, { emit }) {
    const config = reactive({
      buttons: [...props.modelValue.buttons],
      layout: props.modelValue.layout,
      showAskAnother: props.modelValue.showAskAnother
    })

    const buttonsText = ref(props.modelValue.buttons.join('\n'))

    watch(() => props.modelValue, (newValue) => {
      config.buttons = [...newValue.buttons]
      config.layout = newValue.layout
      config.showAskAnother = newValue.showAskAnother
      buttonsText.value = newValue.buttons.join('\n')
    })

    const updateConfig = (evt) => {
      if (evt?.data === ' ') return

      config.buttons = buttonsText.value
        .split('\n')
        .map(text => text.trim())
        .filter(text => text.length > 0)

      emit('update:modelValue', {
        buttons: config.buttons,
        layout: config.layout,
        showAskAnother: config.showAskAnother
      })
    }

    return {
      config,
      buttonsText,
      updateConfig
    }
  }
}
</script>

<style scoped>
.button-list-config {
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

textarea,
select {
  padding: 0.5rem;
  border: 1px solid #E5E7EB;
  border-radius: 4px;
  font-size: 0.875rem;
  font-family: inherit;
}

textarea {
  resize: vertical;
  min-height: 100px;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  font-weight: 500;
  color: #374151;
}

.layout-selector {
  display: flex;
  gap: 1rem;
  margin-top: 0.5rem;
}

.layout-option {
  flex: 1;
  position: relative;
  cursor: pointer;
}

.layout-option input[type="radio"] {
  position: absolute;
  opacity: 0;
  width: 100%;
  height: 100%;
  cursor: pointer;
}

.layout-label {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0.5rem;
  background: #F3F4F6;
  border: 1px solid #E5E7EB;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 500;
  color: #374151;
  transition: all 0.2s;
}

.layout-option input[type="radio"]:checked + .layout-label {
  background: #EBF5FF;
  border-color: #006AFF;
  color: #006AFF;
}

.layout-option:hover .layout-label {
  border-color: #D1D5DB;
}

.layout-option input[type="radio"]:focus + .layout-label {
  outline: 2px solid #006AFF;
  outline-offset: 2px;
}
</style> 