<template>
  <div>
    <search-location
      v-on:coords="getPickUpLocation"/>
  </div>
</template>

<script>
import searchLocation from './searchLocation.vue'

export default {
  components: {
    searchLocation
  },
  methods: {
    getPickUpLocation (location) {
      this.$emit('pickUpLocation', location)
    }
  },
}
</script>

<style>

</style>
