<template>
  <div>
    <label :for="id" class="base-label">
      {{ label }}
    </label>
    <div
        :class="['base-input-wrapper', {'base-input-wrapper_focused': focused}]"
    >
      <input
          :id="id"
          class="base-input"
          v-bind="$attrs"
          :placeholder="$attrs.placeholder"
          :value="value"
          @focus="focusHandler"
          @input="inputHandler"
          @blur="blurHandler"
      >
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: ''
    },

    id: {
      type: String,
      default: ''
    },

    label: {
      type: String,
      default: ''
    }
  },

  data() {
    return {
      focused: false,
    }
  },

  methods: {
    inputHandler(event) {
      this.$emit('input', event.target.value)
    },

    focusHandler() {
      this.focused = true
    },

    blurHandler() {
      this.focused = false
    }
  }
}
</script>

<style scoped>

.base-input-wrapper {
  border: 3px solid #B0B6BE;
  border-radius: 4px;
  width: 100%;
  margin-top: 8px;
}

.base-input-wrapper_focused {
  border-color: gray;
}

.base-input {
  width: 100%;
  height: 30px;
  margin: 0;
  border: none;
  outline: none;
  padding: 3px 6px;
  background: transparent;
}

.base-label {
  color: #6B7686;
  font-size: 18px;
}
</style>