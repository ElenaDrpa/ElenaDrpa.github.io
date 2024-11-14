<script>
import VehicleSelection from './components/VehicleSelection.vue'

export default {
  name: 'App',
  components: {
    VehicleSelection,
  },
}
</script>

<template>
  <div id="app" class="page-container">
    <VehicleSelection />
  </div>
</template>

<style>
.page-container {
  display: flex;
}
</style>
