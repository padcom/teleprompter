<template>
  <dialog ref="dialog">
    <p>
      <label>Edit message</label>
      <input v-model="internalValue" />
    </p>
    <p>
      <button @click="cancel">Cancel</button>
      <button @click="ok">OK</button>
    </p>
  </dialog>
</template>

<script>
export default {
  props: {
    value: { type: String, required: true }
  },
  data() {
    return {
      internalValue: this.value
    }
  },
  methods: {
    open() {
      this.internalValue = this.value
      this.$refs.dialog.showModal()
    },
    cancel() {
      this.$refs.dialog.close()
    },
    ok() {
      this.$refs.dialog.close()
      this.$emit('input', this.internalValue)
    }
  }
}
</script>