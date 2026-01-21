<script setup>
import imagen from '@/assets/imagenes/maxwell-cat.webp'
</script>

<template>
  <div class="imagen-visual">
    <img :src="imagen" alt="Visual" />
  </div>
</template>

<style scoped>
.imagen-visual {
  display: flex;
  justify-content: center;
  align-items: center;
}
img {
  width: 200px;
  height: auto;
}
</style>
