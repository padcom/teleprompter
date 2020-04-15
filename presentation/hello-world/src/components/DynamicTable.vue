<template>
  <table>
    <thead>
      <th v-for="column in columns" :key="column.field">
        {{ column.title }}
      </th>
    </thead>
    <tbody>
      <tr v-for="row in rows" :key="row.id">
        <td v-for="column in columns" :key="column.field">
          <slot :name="column.field" :row="row" :cell="row[column.field]">
            {{ row[column.field] }}
          </slot>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    columns: { type: Array, required: true },
    rows: { type: Array, required: true }
  }
}
</script>
