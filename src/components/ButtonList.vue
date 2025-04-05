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
        backgroundColor: primaryColor
      }"
    >
      <span class="button-text">{{ button }}</span>
      <span v-if="showAiIcon" class="ai-icon">
        <svg width="48" height="48" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path
            fill="currentColor"
            d="M4.52 1.804a1 1 0 0 1 1.96 0l.24 1.192a1 1 0 0 0 .783.785l1.192.238a1 1 0 0 1 0 1.962l-1.192.238a1 1 0 0 0-.785.785L6.48 8.196a1 1 0 0 1-1.962 0L4.28 7.004a1 1 0 0 0-.785-.785L2.303 5.98a1 1 0 0 1 0-1.962l1.192-.238a1 1 0 0 0 .785-.785l.238-1.192h.001Zm9.03 3.88a1 1 0 0 1 1.898 0l.683 2.05a1 1 0 0 0 .633.634l2.051.683a1 1 0 0 1 0 1.898l-2.05.684a1 1 0 0 0-.634.632l-.683 2.05a1 1 0 0 1-1.898 0l-.683-2.05a1.002 1.002 0 0 0-.633-.633l-2.05-.683a1 1 0 0 1 0-1.898l2.05-.683a1 1 0 0 0 .633-.633l.683-2.051Zm-7 8a1 1 0 0 1 1.898 0l.184.55a.999.999 0 0 0 .632.634l.551.183a1 1 0 0 1 0 1.898l-.55.183a1.001 1.001 0 0 0-.634.633l-.183.55a1 1 0 0 1-1.898 0l-.184-.55a1 1 0 0 0-.632-.633l-.55-.183a1 1 0 0 1 0-1.898l.55-.184a1 1 0 0 0 .633-.632l.183-.551Z"
          />
        </svg>
      </span>
    </button>

    <button 
      v-if="showAskAnother"
      class="list-button ask-another"
      :style="{
        borderRadius: `${borderRadius}px`,
        backgroundColor: primaryColor
      }"
    >
      <span class="button-text">Ask another question</span>
      <span v-if="showAiIcon" class="ai-icon">
        <svg width="48" height="48" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path
            fill="currentColor"
            d="M4.52 1.804a1 1 0 0 1 1.96 0l.24 1.192a1 1 0 0 0 .783.785l1.192.238a1 1 0 0 1 0 1.962l-1.192.238a1 1 0 0 0-.785.785L6.48 8.196a1 1 0 0 1-1.962 0L4.28 7.004a1 1 0 0 0-.785-.785L2.303 5.98a1 1 0 0 1 0-1.962l1.192-.238a1 1 0 0 0 .785-.785l.238-1.192h.001Zm9.03 3.88a1 1 0 0 1 1.898 0l.683 2.05a1 1 0 0 0 .633.634l2.051.683a1 1 0 0 1 0 1.898l-2.05.684a1 1 0 0 0-.634.632l-.683 2.05a1 1 0 0 1-1.898 0l-.683-2.05a1.002 1.002 0 0 0-.633-.633l-2.05-.683a1 1 0 0 1 0-1.898l2.05-.683a1 1 0 0 0 .633-.633l.683-2.051Zm-7 8a1 1 0 0 1 1.898 0l.184.55a.999.999 0 0 0 .632.634l.551.183a1 1 0 0 1 0 1.898l-.55.183a1.001 1.001 0 0 0-.634.633l-.183.55a1 1 0 0 1-1.898 0l-.184-.55a1 1 0 0 0-.632-.633l-.55-.183a1 1 0 0 1 0-1.898l.55-.184a1 1 0 0 0 .633-.632l.183-.551Z"
          />
        </svg>
      </span>
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
    showAiIcon: {
      type: Boolean,
      default: true
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
  order: 2;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  min-width: 0;
}

.ai-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  order: 1;
  flex-shrink: 0;
}

.ai-icon svg {
  width: 1.2em;
  height: 1.2em;
}
</style> 