<template>
  <div class="configurator">
    <div class="configurator__preview">
      <div 
        class="preview-container"
        :class="{ 
          'preview-container--responsive': parentConfig.isResponsive,
          'preview-container--drawer': parentConfig.mode === 'drawer'
        }"
        :style="{
          backgroundColor: parentConfig.backgroundColor === 'transparent' ? 'transparent' : parentConfig.backgroundColor,
          borderColor: parentConfig.borderColor === 'none' ? 'transparent' : parentConfig.borderColor,
          borderWidth: parentConfig.borderColor === 'none' ? 0 : `${parentConfig.borderWidth}px`,
          borderRadius: `${parentConfig.borderRadius}px`,
          borderStyle: parentConfig.borderColor === 'none' ? 'none' : 'solid',
          width: !parentConfig.isResponsive ? `${parentConfig.width}px` : undefined,
          height: !parentConfig.isResponsive ? `${parentConfig.height}px` : undefined
        }"
      >
        <div class="header-container" style="display: flex; align-items: center; gap: 1rem;">
          <template v-if="parentConfig.showThumbnail">
            <div class="thumbnail">
              <img src="@/assets/avatar.svg" alt="Thumbnail" />
            </div>
          </template>
          <h3 v-if="parentConfig.title" class="title">{{ parentConfig.title }}</h3>
        </div>
        
        <div class="modules-container">
          <template v-for="module in enabledModules" :key="module.id">
            <component 
              :is="module.type" 
              v-bind="getModuleProps(module)"
              :primary-color="parentConfig.primaryColor"
              :border-radius="parentConfig.borderRadius"
            />
          </template>
        </div>
      </div>
    </div>

    <div class="configurator__settings">
      <div class="settings-section">
        <h2>Parent Container Settings</h2>
        <div class="settings-group">
          <label class="checkbox-label">
            <input 
              type="checkbox" 
              v-model="parentConfig.isResponsive"
            > Responsive Container
          </label>

          <template v-if="!parentConfig.isResponsive">
            <div class="form-group">
              <label>Width (px)</label>
              <input 
                type="number" 
                v-model.number="parentConfig.width"
                min="0"
              >
            </div>
            <div class="form-group">
              <label>Height (px)</label>
              <input 
                type="number" 
                v-model.number="parentConfig.height"
                min="0"
              >
            </div>
          </template>
          
          <label class="checkbox-label">
            <input 
              type="checkbox" 
              v-model="parentConfig.showThumbnail"
            > Show Thumbnail
          </label>
          
          <div class="form-group">
            <label>Title</label>
            <input type="text" v-model="parentConfig.title">
          </div>

          <div class="form-group">
            <label>Primary Color</label>
            <div class="color-with-text">
              <div class="color-swatch">
                <input 
                  type="color" 
                  v-model="parentConfig.primaryColor"
                >
              </div>
              <input 
                type="text" 
                v-model="parentConfig.primaryColor"
                class="color-text"
              >
            </div>
          </div>

          <div class="form-group">
            <label>Border</label>
            <div class="border-controls">
              <div class="color-with-text">
                <div 
                  class="color-swatch"
                  :class="{ 'checker-pattern': parentConfig.borderColor === 'none' }"
                >
                  <input 
                    type="color" 
                    v-model="parentConfig.borderColor"
                    :disabled="parentConfig.borderColor === 'none'"
                    :class="{ 'transparent': parentConfig.borderColor === 'none' }"
                  >
                </div>
                <input 
                  type="text" 
                  v-model="parentConfig.borderColor"
                  :disabled="parentConfig.borderColor === 'none'"
                  :class="{ 'transparent': parentConfig.borderColor === 'none' }"
                  class="color-text"
                >
              </div>
              <button 
                class="secondary-button"
                @click="toggleBorderColor"
              >
                {{ parentConfig.borderColor === 'none' ? 'Set Color' : 'None' }}
              </button>
              <input 
                type="number" 
                v-model.number="parentConfig.borderWidth"
                min="0"
                :disabled="parentConfig.borderColor === 'none'"
                class="border-width"
              >
              <span class="unit-label">px</span>
            </div>
          </div>

          <div class="form-group">
            <label>Border Radius (px)</label>
            <input type="number" v-model.number="parentConfig.borderRadius" min="0">
          </div>

          <div class="form-group">
            <label>Background Color</label>
            <div class="color-with-button">
              <div 
                class="color-input-wrapper"
                :class="{ 'checker-pattern': parentConfig.backgroundColor === 'transparent' }"
              >
                <input 
                  type="color" 
                  v-model="parentConfig.backgroundColor"
                  :disabled="parentConfig.backgroundColor === 'transparent'"
                  :class="{ 'transparent': parentConfig.backgroundColor === 'transparent' }"
                >
              </div>
              <button 
                class="secondary-button"
                @click="toggleBackgroundColor"
              >
                {{ parentConfig.backgroundColor === 'transparent' ? 'Set Color' : 'Transparent' }}
              </button>
            </div>
          </div>

          <div class="form-group">
            <label>Mode</label>
            <select v-model="parentConfig.mode">
              <option value="block">Block</option>
              <option value="drawer">Drawer</option>
            </select>
          </div>
        </div>
      </div>

      <div class="settings-section" v-if="modules[0].enabled">
        <h2>Simple Button Settings</h2>
        <SimpleButtonConfig v-model="modules[0].config" />
      </div>

      <div class="settings-section" v-if="modules[1].enabled">
        <h2>Question List Settings</h2>
        <ButtonListConfig v-model="modules[1].config" />
      </div>

      <div class="settings-section" v-if="modules[2].enabled">
        <h2>Question Field Settings</h2>
        <TextFieldConfig v-model="modules[2].config" />
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, computed } from 'vue'
import SimpleButton from './SimpleButton.vue'
import SimpleButtonConfig from './SimpleButtonConfig.vue'
import ButtonList from './ButtonList.vue'
import ButtonListConfig from './ButtonListConfig.vue'
import TextField from './TextField.vue'
import TextFieldConfig from './TextFieldConfig.vue'

export default {
  name: 'EntrypointConfigurator',
  
  components: {
    SimpleButton,
    SimpleButtonConfig,
    ButtonList,
    ButtonListConfig,
    TextField,
    TextFieldConfig
  },

  setup() {
    const parentConfig = reactive({
      isResponsive: true,
      width: 400,
      height: 600,
      showThumbnail: true,
      title: 'Have questions about this product?',
      primaryColor: '#006AFF',
      borderColor: '#E5E7EB',
      borderWidth: 1,
      borderRadius: 12,
      backgroundColor: '#FFFFFF',
      mode: 'block'
    })

    const modules = ref([
      {
        id: 1,
        type: 'SimpleButton',
        enabled: true,
        config: {
          text: 'Ask an AI expert',
          showAiIcon: true
        }
      },
      {
        id: 2,
        type: 'ButtonList',
        enabled: true,
        config: {
          buttons: ['Lorem ipsum dolor sit amet', 'Consectetur adipiscing elit', 'Sed do eiusmod tempor'],
          layout: 'horizontal',
          showAiIcon: false,
          showAskAnother: true
        }
      },
      {
        id: 3,
        type: 'TextField',
        enabled: true,
        config: {
          placeholder: 'Ask a question'
        }
      }
    ])

    const displayOrder = ref(modules.value.map((_, index) => index))

    const enabledModules = computed(() => 
      displayOrder.value
        .map(index => modules.value[index])
        .filter(m => m.enabled)
    )

    const getModuleLabel = (type) => {
      const labels = {
        SimpleButton: 'Simple Button',
        ButtonList: 'Question List',
        TextField: 'Question Field'
      }
      return labels[type] || type
    }

    const getModuleProps = (module) => {
      return {
        ...module.config
      }
    }

    const moveModule = (index, direction) => {
      const currentDisplayIndex = displayOrder.value.indexOf(index)
      const newDisplayIndex = currentDisplayIndex + direction
      
      if (newDisplayIndex < 0 || newDisplayIndex >= displayOrder.value.length) return
      
      const temp = displayOrder.value[currentDisplayIndex]
      displayOrder.value[currentDisplayIndex] = displayOrder.value[newDisplayIndex]
      displayOrder.value[newDisplayIndex] = temp
    }

    const toggleBorderColor = () => {
      parentConfig.borderColor = parentConfig.borderColor === 'none' 
        ? '#E5E7EB' 
        : 'none'
    }

    const toggleBackgroundColor = () => {
      parentConfig.backgroundColor = parentConfig.backgroundColor === 'transparent'
        ? '#FFFFFF'
        : 'transparent'
    }

    return {
      parentConfig,
      modules,
      enabledModules,
      getModuleLabel,
      getModuleProps,
      moveModule,
      toggleBorderColor,
      toggleBackgroundColor,
      displayOrder
    }
  }
}
</script>

<style scoped>
.configurator {
  display: grid;
  grid-template-rows: 1fr 1fr;
  gap: 2rem;
  padding: 2rem;
  height: 100vh;
  background: #F3F4F6;
}

.configurator__preview {
  background: linear-gradient(to bottom, #E5E7EB, #FFFFFF);
  border-radius: 8px;
  padding: 2rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.configurator__settings {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr 1fr;
  gap: 1rem;
  height: 100%;
}

.settings-section {
  background: white;
  border-radius: 8px;
  padding: 1.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  overflow-y: auto;
  max-height: 100%;
}

.settings-section h2 {
  margin: 0 0 1rem;
  font-size: 1.25rem;
  font-weight: 600;
}

.settings-group {
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

input[type="text"],
input[type="number"],
select {
  padding: 0.5rem;
  border: 1px solid #E5E7EB;
  border-radius: 4px;
  font-size: 0.875rem;
}

input[type="color"] {
  width: 100%;
  height: 38px;
  padding: 0;
  border: 1px solid #E5E7EB;
  border-radius: 4px;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  font-weight: 500;
  color: #374151;
}

.color-with-button {
  display: flex;
  gap: 0.5rem;
}

.color-with-button input[type="color"] {
  flex: 1;
}

.secondary-button {
  padding: 0.5rem 1rem;
  background: #F3F4F6;
  border: 1px solid #E5E7EB;
  border-radius: 4px;
  font-size: 0.875rem;
  color: #374151;
  cursor: pointer;
}

.secondary-button:hover {
  background: #E5E7EB;
}

.modules-settings {
  display: none;
}

.module-settings {
  height: 100%;
}

.preview-container {
  max-height: 100%;
  overflow-y: auto;
  min-width: 300px;
  min-height: 200px;
  padding: 1.5rem;
  background: #fff;
  border: 1px solid #E5E7EB;
  border-radius: 8px;
}

.preview-container--responsive {
  width: 100%;
}

.preview-container--drawer {
  position: relative;
  margin-left: auto;
  height: 100%;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}

.header-container {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.thumbnail {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  overflow: hidden;
  margin-bottom: 1rem;
}

.thumbnail img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.title {
  margin: 0 0 1rem;
  font-size: 1.25rem;
  font-weight: 600;
  color: #111827;
}

.modules-container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.border-controls {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.border-controls .color-with-button {
  flex: 1;
}

.border-width {
  width: 70px;
}

.unit-label {
  font-size: 0.875rem;
  color: #374151;
}

.color-input-wrapper {
  position: relative;
  flex: 1;
  height: 38px;
  border: 1px solid #E5E7EB;
  border-radius: 4px;
  overflow: hidden;
}

.color-input-wrapper input[type="color"] {
  position: relative;
  width: 100%;
  height: 100%;
  border: none;
  background: none;
}

.color-input-wrapper input[type="color"].transparent {
  opacity: 0;
}

.checker-pattern {
  background-image: linear-gradient(45deg, #E5E7EB 25%, transparent 25%),
    linear-gradient(-45deg, #E5E7EB 25%, transparent 25%),
    linear-gradient(45deg, transparent 75%, #E5E7EB 75%),
    linear-gradient(-45deg, transparent 75%, #E5E7EB 75%);
  background-size: 16px 16px;
  background-position: 0 0, 0 8px, 8px -8px, -8px 0px;
  background-color: white;
}

.color-with-text {
  display: flex;
  gap: 0.5rem;
  align-items: center;
}

.color-swatch {
  width: 38px;
  height: 38px;
  border: 1px solid #E5E7EB;
  border-radius: 4px;
  overflow: hidden;
  flex-shrink: 0;
}

.color-swatch input[type="color"] {
  width: 100%;
  height: 100%;
  padding: 0;
  border: none;
  cursor: pointer;
}

.color-text {
  flex: 1;
  font-family: monospace;
  text-transform: uppercase;
  width: 100px;
}

.color-swatch.transparent {
  opacity: 0.5;
}

.color-text.transparent {
  opacity: 0.5;
  background-color: #F3F4F6;
}
</style>