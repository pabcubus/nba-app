<template>
  <div v-if="items.length > 0">
    <b-table striped hover :items="items"></b-table>
  </div>
  <div v-else>
    No matches found...
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      default: () => []
    }
  }
}
</script>
