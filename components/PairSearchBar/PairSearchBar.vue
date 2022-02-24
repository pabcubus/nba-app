<template>
  <b-form-group id="input-group-1" label="Search Inches" label-for="input-1">
    <b-form-input id="input-1" v-model="searchTerm" type="text" placeholder="Enter number" @input="termChange">
    </b-form-input>
  </b-form-group>
</template>

<script>
export default {
  props: {
    searchTerm: {
      type: String,
      default: () => ''
    }
  },
  methods: {
    termChange(event) {
      this.$emit('termChange', event)
    }
  }
}
</script>
