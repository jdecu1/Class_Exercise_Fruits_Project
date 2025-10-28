<script setup>
// Import your custom component
import ProductListOptions from "./components/ProductListOptions.vue";
</script>

<template>
  <!-- Center wrapper for layout -->
  <div class="wrapper">
    <!-- Use your custom component -->
    <ProductListOptions />
  </div>
</template>

<style scoped>
/* Center the component on the page */
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  min-width: 100vw;
  background-color: #f8fafc;
  margin: 0;
}
</style>
