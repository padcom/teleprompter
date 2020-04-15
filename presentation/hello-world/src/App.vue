<template>
  <PageTemplate id="app">
    <template slot="header">
      <h1>{{ message }}</h1>
    </template>

    <dynamic-table :columns="columns" :rows="people">
      <template #name="{ cell }">
        <i style="color: red; font-weight: bold">{{ cell }}</i>
      </template>
    </dynamic-table>

    <p>
      <EditorDialog ref="dialog" v-model="message" />
      <button @click="showDialog">Click me!</button>
    </p>
    <p><input v-model="message"></p>

    {{ fullName }}

    <h1>{{ f1 }}</h1>

  </PageTemplate>
</template>

<script>
import PageTemplate from '@/components/PageTemplate'
import DynamicTable from '@/components/DynamicTable'
import EditorDialog from '@/components/EditorDialog'

export default {
  components: {
    PageTemplate,
    DynamicTable,
    EditorDialog,
  },
  data() {
    return {
      firstName: 'John',
      lastName: 'Doe',
      f1: {
        f2: undefined
      },
      message: 'Hello, world!',
      columns: [
        { field: 'name', title: 'User' },
        { field: 'age', title: 'Age' }
      ],
      people: [
        { id: 1, name: 'John Doe', age: 32 },
        { id: 2, name: 'Jane Smith', age: 22 },
        { id: 3, name: 'Yoda', age: 700 }
      ]    }
  },
  // watch: {
  //   firstName(newValue) {
  //     this.fullName = newValue + ' ' + this.lastName
  //   }
  // },
  computed: {
    fullName() {
      return this.firstName + ' ' + this.lastName
    }
  },
  methods: {
    showDialog() {
      this.$refs.dialog.open()
    },
    closeDialog() {
      this.$refs.dialog.close()
    }
  }
}
</script>

<style lang="scss">
body {
  background-color: red;
  & #app {
    font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  }
}
</style>
