<template>
  <div>
    <label>{{ label }}</label>
    <input v-model="internalValue">
    <button @click="$emit('input', internalValue)">Save</button>
  </div>
</template>

<script>
export default {
  props: {
    value: { type: String, required: true },
    label: { type: String, required: false, default: 'Label' }
  },
  data() {
    return {
      internalValue: this.value
    }
  },
  watch: {
    value(newValue) {
      this.internalValue = newValue
    }
  }
}
</script>