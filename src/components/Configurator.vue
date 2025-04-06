<template>
  <div class="configurator">
    <div class="configurator__preview">
      <div 
        class="preview-container"
        :class="{ 
          'preview-container--responsive': !parentConfig.isFixedSize,
          'preview-container--drawer': parentConfig.mode === 'drawer'
        }"
        :style="{
          backgroundColor: parentConfig.backgroundColor === 'transparent' ? 'transparent' : parentConfig.backgroundColor,
          borderColor: parentConfig.borderColor === 'none' ? 'transparent' : parentConfig.borderColor,
          borderWidth: parentConfig.borderColor === 'none' ? 0 : `${parentConfig.borderWidth}px`,
          borderRadius: `${parentConfig.borderRadius}px`,
          borderStyle: parentConfig.borderColor === 'none' ? 'none' : 'solid',
          width: parentConfig.isFixedSize ? `${parentConfig.width}px` : undefined,
          height: parentConfig.isFixedSize ? `${parentConfig.height}px` : undefined
        }"
      >
        <div v-if="parentConfig.mode === 'drawer'" class="drawer-tab" :style="{ backgroundColor: parentConfig.backgroundColor === 'transparent' ? 'transparent' : parentConfig.backgroundColor }">
          <svg class="drawer-tab-icon" width="16" height="16" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path
            :fill="parentConfig.primaryColor"
            d="M4.52 1.804a1 1 0 0 1 1.96 0l.24 1.192a1 1 0 0 0 .783.785l1.192.238a1 1 0 0 1 0 1.962l-1.192.238a1 1 0 0 0-.785.785L6.48 8.196a1 1 0 0 1-1.962 0L4.28 7.004a1 1 0 0 0-.785-.785L2.303 5.98a1 1 0 0 1 0-1.962l1.192-.238a1 1 0 0 0 .785-.785l.238-1.192h.001Zm9.03 3.88a1 1 0 0 1 1.898 0l.683 2.05a1 1 0 0 0 .633.634l2.051.683a1 1 0 0 1 0 1.898l-2.05.684a1 1 0 0 0-.634.632l-.683 2.05a1 1 0 0 1-1.898 0l-.683-2.05a1.002 1.002 0 0 0-.633-.633l-2.05-.683a1 1 0 0 1 0-1.898l2.05-.683a1 1 0 0 0 .633-.633l.683-2.051Zm-7 8a1 1 0 0 1 1.898 0l.184.55a.999.999 0 0 0 .632.634l.551.183a1 1 0 0 1 0 1.898l-.55.183a1.001 1.001 0 0 0-.634.633l-.183.55a1 1 0 0 1-1.898 0l-.184-.55a1 1 0 0 0-.632-.633l-.55-.183a1 1 0 0 1 0-1.898l.55-.184a1 1 0 0 0 .633-.632l.183-.551Z"
            />
          </svg>
          <span class="drawer-tab-text">Ask</span>
        </div>
        <div class="header-container" style="display: flex; align-items: center; gap: 1rem;">
          <template v-if="parentConfig.showThumbnail">
            <div class="thumbnail">
              <img src="@/assets/thumbnail.gif" alt="Thumbnail" />
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
              :text-color="buttonTextColor"
            />
          </template>
        </div>
      </div>
    </div>

    <div class="configurator__settings">
      <div class="settings-section" style="grid-row: 1 / -1;">
        <div class="settings-header">
          <h2>Ava Entry Point</h2>
          <div class="mode-selector">
            <label class="mode-option">
              <input 
                type="radio" 
                v-model="isAdvancedMode" 
                :value="false"
              >
              <span class="mode-label">Easy</span>
            </label>
            <label class="mode-option">
              <input 
                type="radio" 
                v-model="isAdvancedMode" 
                :value="true"
              >
              <span class="mode-label">Advanced</span>
            </label>
          </div>
        </div>

        <div v-if="!isAdvancedMode" class="easy-mode">
          <button 
            class="preset-button"
            @click="applyPreset('simpleButton')"
          >
            Simple Button
          </button>
          <button 
            class="preset-button"
            @click="applyPreset('faq')"
          >
            FAQ
          </button>
          <button 
            class="preset-button"
            @click="applyPreset('drawer')"
          >
            Drawer
          </button>
        </div>

        <div v-show="isAdvancedMode" class="settings-group">
          <div class="container-size-controls">
            <label class="checkbox-label" style="margin-top:20px;">
              <input 
                type="checkbox" 
                v-model="parentConfig.isFixedSize"
              > Set Fixed Size
            </label>

            <div class="form-group">
              <label>Width (px)</label>
              <input 
                type="number" 
                v-model.number="parentConfig.width"
                min="0"
                :disabled="!parentConfig.isFixedSize"
              >
            </div>
            <div class="form-group">
              <label>Height (px)</label>
              <input 
                type="number" 
                v-model.number="parentConfig.height"
                min="0"
                :disabled="!parentConfig.isFixedSize"
              >
            </div>
          </div>

          <div class="form-group">
            <label>Mode</label>
            <div class="mode-selector">
              <label class="mode-option">
                <input 
                  type="radio" 
                  v-model="parentConfig.mode" 
                  value="block"
                >
                <span class="mode-label">Block</span>
              </label>
              <label class="mode-option">
                <input 
                  type="radio" 
                  v-model="parentConfig.mode" 
                  value="drawer"
                >
                <span class="mode-label">Drawer</span>
              </label>
            </div>
          </div>
          
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
            <div class="radius-control">
              <input 
                type="range" 
                v-model.number="parentConfig.borderRadius"
                min="0"
                max="32"
                class="radius-slider"
              >
              <input 
                type="number" 
                v-model.number="parentConfig.borderRadius"
                min="0"
                max="32"
                class="radius-number"
              >
            </div>
          </div>

          <div class="form-group">
            <label>Background Color</label>
            <div class="color-with-text">
              <div 
                class="color-swatch"
                :class="{ 'checker-pattern': parentConfig.backgroundColor === 'transparent' }"
              >
                <input 
                  type="color" 
                  v-model="parentConfig.backgroundColor"
                  :disabled="parentConfig.backgroundColor === 'transparent'"
                  :class="{ 'transparent': parentConfig.backgroundColor === 'transparent' }"
                >
              </div>
              <input 
                type="text" 
                v-model="parentConfig.backgroundColor"
                :disabled="parentConfig.backgroundColor === 'transparent'"
                :class="{ 'transparent': parentConfig.backgroundColor === 'transparent' }"
                class="color-text"
              >
              <button 
                class="secondary-button"
                @click="toggleBackgroundColor"
              >
                {{ parentConfig.backgroundColor === 'transparent' ? 'Set Color' : 'Transparent' }}
              </button>
            </div>
          </div>
        </div>
      </div>

      <div v-show="isAdvancedMode" class="settings-section">
        <div class="module-header">
          <label class="toggle-label">
            <input 
              type="checkbox" 
              v-model="modules[0].enabled"
            >
            <span class="toggle-text">Simple Button</span>
          </label>
          <div class="module-controls">
            <button 
              class="icon-button"
              @click="moveModule(0, -1)"
              :disabled="!modules[0].enabled || displayOrder.indexOf(0) === 0"
            >↑</button>
            <button 
              class="icon-button"
              @click="moveModule(0, 1)"
              :disabled="!modules[0].enabled || displayOrder.indexOf(0) === modules.length - 1"
            >↓</button>
          </div>
        </div>
        <div 
          class="module-config"
          :class="{ 'module-config--disabled': !modules[0].enabled }"
        >
          <SimpleButtonConfig v-model="modules[0].config" />
        </div>
      </div>

      <div v-show="isAdvancedMode" class="settings-section">
        <div class="module-header">
          <label class="toggle-label">
            <input 
              type="checkbox" 
              v-model="modules[1].enabled"
            >
            <span class="toggle-text">Question List</span>
          </label>
          <div class="module-controls">
            <button 
              class="icon-button"
              @click="moveModule(1, -1)"
              :disabled="!modules[1].enabled || displayOrder.indexOf(1) === 0"
            >↑</button>
            <button 
              class="icon-button"
              @click="moveModule(1, 1)"
              :disabled="!modules[1].enabled || displayOrder.indexOf(1) === modules.length - 1"
            >↓</button>
          </div>
        </div>
        <div 
          class="module-config"
          :class="{ 'module-config--disabled': !modules[1].enabled }"
        >
          <ButtonListConfig v-model="modules[1].config" />
        </div>
      </div>

      <div v-show="isAdvancedMode" class="settings-section">
        <div class="module-header">
          <label class="toggle-label">
            <input 
              type="checkbox" 
              v-model="modules[2].enabled"
            >
            <span class="toggle-text">Question Field</span>
          </label>
          <div class="module-controls">
            <button 
              class="icon-button"
              @click="moveModule(2, -1)"
              :disabled="!modules[2].enabled || displayOrder.indexOf(2) === 0"
            >↑</button>
            <button 
              class="icon-button"
              @click="moveModule(2, 1)"
              :disabled="!modules[2].enabled || displayOrder.indexOf(2) === modules.length - 1"
            >↓</button>
          </div>
        </div>
        <div 
          class="module-config"
          :class="{ 'module-config--disabled': !modules[2].enabled }"
        >
          <TextFieldConfig v-model="modules[2].config" />
        </div>
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

const calculateBrightness = (hexColor) => {
  // Remove # if present
  const hex = hexColor.replace('#', '')
  
  // Convert hex to RGB
  const r = parseInt(hex.substring(0, 2), 16)
  const g = parseInt(hex.substring(2, 4), 16)
  const b = parseInt(hex.substring(4, 6), 16)
  
  // Calculate brightness using perceived brightness formula
  // (R * 299 + G * 587 + B * 114) / 1000
  const brightness = Math.round((r * 299 + g * 587 + b * 114) / 1000)
  
  // Ensure the result is between 0 and 255
  return Math.min(255, Math.max(0, brightness))
}

const PRESETS = {
  simpleButton: {
    mode: 'block',
    isFixedSize: true,
    width: 400,
    height: 170,
    showThumbnail: true,
    title: 'Have questions about this product?',
    borderRadius: 12,
    modules: [true, false, false] // Simple Button, Question List, Question Field
  },
  faq: {
    mode: 'block',
    isFixedSize: false,
    showThumbnail: false,
    title: '',
    borderRadius: 12,
    modules: [false, true, true]
  },
  drawer: {
    mode: 'drawer',
    isFixedSize: true,
    width: 200,
    height: 122,
    showThumbnail: false,
    title: 'Lorem ipsum dolor sit amet?',
    borderRadius: 0,
    modules: [false, false, false]
  }
}

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
    const isAdvancedMode = ref(false)

    const parentConfig = reactive({
      isFixedSize: true,
      width: 400,
      height: 170,
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
        enabled: false,
        config: {
          buttons: ['Lorem ipsum dolor sit amet', 'Consectetur adipiscing elit'],
          layout: 'horizontal',
          showAskAnother: true
        }
      },
      {
        id: 3,
        type: 'TextField',
        enabled: false,
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

    const isResponsive = computed(() => !parentConfig.isFixedSize)

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

    // Add computed property for text color
    const buttonTextColor = computed(() => {
      const brightness = calculateBrightness(parentConfig.primaryColor)
      return brightness > 127 ? '#000000' : '#FFFFFF'
    })

    const applyPreset = (preset) => {
      parentConfig.mode = PRESETS[preset].mode
      parentConfig.showThumbnail = PRESETS[preset].showThumbnail
      parentConfig.title = PRESETS[preset].title
      parentConfig.isFixedSize = PRESETS[preset].isFixedSize
      parentConfig.width = PRESETS[preset].width
      parentConfig.height = PRESETS[preset].height
      parentConfig.borderRadius = PRESETS[preset].borderRadius  
      PRESETS[preset].modules.forEach((enabled, index) => {
        modules.value[index].enabled = enabled
      })
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
      displayOrder,
      isResponsive,
      buttonTextColor,
      isAdvancedMode,
      applyPreset
    }
  }
}
</script>

<style scoped>
.configurator {
  display: grid;
  grid-template-rows: min(auto, 300px) 1fr;
  gap: 2rem;
  padding: 2rem;
  height: 100vh;
  background: #F3F4F6;
  overflow: hidden;
}

.configurator__preview {
  background: linear-gradient(to bottom, #E5E7EB, #FFFFFF);
  border-radius: 8px;
  padding: 2rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: center;
  align-items: flex-start;
  position: relative;
  overflow: hidden;
}

.configurator__settings {
  display: grid;
  grid-template: auto auto auto auto / 1fr 1fr;
  gap: 1rem;
  height: 100%;

  background: white;
  border-radius: 8px;
  padding: 1.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  overflow-y: auto;
  max-height: 100%;

  h2 {
    margin: 0;
    font-size: 1.55rem;
    font-weight: 600;
  }
}

.settings-section {
  padding: 1rem;
  border: 1px solid #E5E7EB;
  border-radius: 8px;
} 

.settings-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 1.5rem;
}

.settings-header h2 {
  margin: 0;
}

.easy-mode {
  display: grid;
  gap: 1rem;
  padding: 2rem 0;
}

.preset-button {
  padding: 2rem;
  font-size: 1.25rem;
  font-weight: 500;
  color: #374151;
  background: #F3F4F6;
  border: 1px solid #E5E7EB;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
}

.preset-button:hover {
  background: #EBF5FF;
  border-color: #006AFF;
  color: #006AFF;
}

.mode-selector {
  display: flex;
  gap: 0.5rem;
}

.mode-option {
  position: relative;
}

.mode-option input[type="radio"] {
  position: absolute;
  opacity: 0;
  width: 100%;
  height: 100%;
  cursor: pointer;
}

.mode-label {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0.5rem 0.75rem;
  background: #F3F4F6;
  border: 1px solid #E5E7EB;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 500;
  color: #374151;
  transition: all 0.2s;
}

.mode-option input[type="radio"]:checked + .mode-label {
  background: #EBF5FF;
  border-color: #006AFF;
  color: #006AFF;
}

.mode-option:hover .mode-label {
  border-color: #D1D5DB;
}

.mode-option input[type="radio"]:focus + .mode-label {
  outline: 2px solid #006AFF;
  outline-offset: 2px;
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
  padding: 1.5rem;
  background: #fff;
  border: 1px solid #E5E7EB;
  border-radius: 8px;
  transition: transform 0.3s ease-in-out;
}

.preview-container--responsive {
  width: 100%;
}

.preview-container--drawer {
  overflow: visible;
  position: absolute;
  right: 0;
  top: 0;
  height: 100%;
  margin: 0;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
  transform: translateX(calc(100% - 8px));
}

.preview-container--drawer:hover {
  transform: translateX(0);
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
  position: relative;
}

.color-swatch input[type="color"] {
  width: 100%;
  height: 100%;
  padding: 0;
  border: none;
  cursor: pointer;
  position: relative;
  z-index: 1;
}

.color-swatch input[type="color"].transparent {
  opacity: 0;
  position: absolute;
  pointer-events: none;
}

.color-text {
  flex: 1;
  font-family: monospace;
  text-transform: uppercase;
  width: 100px;
}

.color-text.transparent {
  opacity: 0.5;
  background-color: #F3F4F6;
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

.module-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.toggle-label {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
}

.toggle-label input[type="checkbox"] {
  width: 24px;
  height: 24px;
  margin: 0;
  cursor: pointer;
  accent-color: #006AFF;
}

.toggle-text {
  font-size: 1.25rem;
  font-weight: 600;
  color: #111827;
  user-select: none;
}

.module-controls {
  display: flex;
  gap: 0.25rem;
}

.icon-button {
  padding: 0.25rem 0.5rem;
  background: white;
  border: 1px solid #E5E7EB;
  border-radius: 4px;
  font-size: 0.875rem;
  color: #374151;
  cursor: pointer;
  transition: all 0.2s;
}

.icon-button:hover:not(:disabled) {
  background: #F3F4F6;
  border-color: #D1D5DB;
}

.icon-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.module-config {
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid #E5E7EB;
  transition: opacity 0.2s, filter 0.2s;
}

.module-config--disabled {
  opacity: 0.5;
  filter: grayscale(100%);
  pointer-events: none;
}

.drawer-tab {
  position: absolute;
  left: -40px;
  top: 50%;
  transform: translateY(-50%);
  width: 40px;
  height: 120px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  border-radius: 8px 0 0 8px;
  cursor: pointer;
}

.drawer-tab-text {
  writing-mode: vertical-lr;
  transform: rotate(180deg);
  font-size: 1rem;
  font-weight: 500;
  letter-spacing: 0.05em;
  text-transform: uppercase;
}

.drawer-tab-icon {
  transform: rotate(90deg);
}

.container-size-controls {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.container-size-controls .form-group {
  flex: 1;
  margin: 0;
}

.container-size-controls input[type="number"] {
  width: 100%;
}

.container-size-controls input[type="number"]:disabled {
  background-color: #F3F4F6;
  opacity: 0.5;
  cursor: not-allowed;
}

.container-size-controls .checkbox-label {
  min-width: 160px;
}

.radius-control {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.radius-slider {
  flex: 1;
  height: 2px;
  background: #E5E7EB;
  border-radius: 2px;
  appearance: none;
  outline: none;
}

.radius-slider::-webkit-slider-thumb {
  appearance: none;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: #006AFF;
  cursor: pointer;
  border: none;
  margin-top: -6px;
}

.radius-slider::-moz-range-thumb {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: #006AFF;
  cursor: pointer;
  border: none;
}

.radius-slider::-webkit-slider-runnable-track {
  height: 2px;
  background: #E5E7EB;
  border-radius: 2px;
  border: none;
}

.radius-slider::-moz-range-track {
  height: 2px;
  background: #E5E7EB;
  border-radius: 2px;
  border: none;
}

.radius-number {
  width: 60px;
  text-align: center;
  padding: 0.5rem;
  border: 1px solid #E5E7EB;
  border-radius: 4px;
  font-size: 0.875rem;
}

.radius-number::-webkit-inner-spin-button {
  opacity: 1;
  height: 24px;
}
</style>