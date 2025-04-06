<template>
  <div 
    class="button-list"
    :class="{ 'button-list--vertical': layout === 'vertical' }"
  >
    <button 
      v-for="(button, index) in buttons"
      :key="index"
      class="list-button"
      :style="{
        borderRadius: `${borderRadius}px`,
        backgroundColor: primaryColor,
        color: textColor
      }"
    >
      <span class="button-text">{{ button }}</span>
    </button>

    <button 
      v-if="showAskAnother"
      class="list-button ask-another"
      :style="{
        borderRadius: `${borderRadius}px`,
        backgroundColor: primaryColor,
        color: textColor
      }"
    >
      <span class="button-text">Ask another question</span>
    </button>
  </div>
</template>

<script>
export default {
  name: 'ButtonList',
  
  props: {
    buttons: {
      type: Array,
      default: () => []
    },
    layout: {
      type: String,
      default: 'horizontal',
      validator: value => ['horizontal', 'vertical'].includes(value)
    },
    showAskAnother: {
      type: Boolean,
      default: true
    },
    primaryColor: {
      type: String,
      default: '#007AFF'
    },
    borderRadius: {
      type: Number,
      default: 8
    },
    textColor: {
      type: String,
      default: '#FFFFFF'
    }
  }
}
</script>

<style scoped>
.button-list {
  display: flex;
  gap: 0.5rem;
  position: relative;
}

.button-list--vertical {
  flex-direction: column;
  flex-wrap: wrap;
}

/* Only apply these styles when not in vertical mode */
.button-list:not(.button-list--vertical) {
  width: 100%;
  overflow: hidden;
  mask-image: linear-gradient(to right, 
    black 80%, 
    transparent 100%
  );
  -webkit-mask-image: linear-gradient(to right, 
    black 80%, 
    transparent 100%
  );
}

.button-list:not(.button-list--vertical) .list-button {
  flex: 0 0 40%;
}

.button-list:not(.button-list--vertical) .list-button:nth-child(3) {
  flex: 0 0 20%;
}

.button-list:not(.button-list--vertical) .list-button:nth-child(n+4) {
  flex: 0 0 40%;
  visibility: hidden;
  position: absolute;
}

.list-button {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1rem;
  border: none;
  color: white;
  font-size: 0.875rem;
  font-weight: 500;
  cursor: pointer;
  transition: opacity 0.2s;
  white-space: nowrap;
  min-width: 0; /* Allow truncation */
}

.list-button:hover {
  opacity: 0.9;
}

.list-button.ask-another {
  opacity: 0.9;
}

.button-text {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  min-width: 0;
}
</style> 