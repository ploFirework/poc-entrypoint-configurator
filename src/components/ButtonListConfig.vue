<template>
  <div class="button-list-config">
    <div class="form-group">
      <label>Questions (one per line)</label>
      <textarea
        v-model="buttonsText"
        @input="updateConfig"
        rows="4"
        placeholder="Enter button text, one per line"
      ></textarea>
    </div>

    <div class="form-group">
      <label>Layout</label>
      <select 
        v-model="config.layout"
        @change="updateConfig"
      >
        <option value="horizontal">Horizontal</option>
        <option value="vertical">Vertical</option>
      </select>
    </div>

    <label class="checkbox-label">
      <input 
        type="checkbox" 
        v-model="config.showAiIcon"
        @change="updateConfig"
      > Show AI Icon
    </label>

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
        showAiIcon: true,
        showAskAnother: true
      })
    }
  },

  emits: ['update:modelValue'],

  setup(props, { emit }) {
    const config = reactive({
      buttons: [...props.modelValue.buttons],
      layout: props.modelValue.layout,
      showAiIcon: props.modelValue.showAiIcon,
      showAskAnother: props.modelValue.showAskAnother
    })

    const buttonsText = ref(props.modelValue.buttons.join('\n'))

    watch(() => props.modelValue, (newValue) => {
      config.buttons = [...newValue.buttons]
      config.layout = newValue.layout
      config.showAiIcon = newValue.showAiIcon
      config.showAskAnother = newValue.showAskAnother
      buttonsText.value = newValue.buttons.join('\n')
    })

    const updateConfig = () => {
      config.buttons = buttonsText.value
        .split('\n')
        .map(text => text.trim())
        .filter(text => text.length > 0)

      emit('update:modelValue', {
        buttons: config.buttons,
        layout: config.layout,
        showAiIcon: config.showAiIcon,
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
</style> 