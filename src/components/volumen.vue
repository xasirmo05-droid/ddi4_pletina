<script setup>
import { ref, watch } from 'vue'
const props = defineProps({ audio: Object })
const emit = defineEmits(['update:volume'])

const volumen = ref(1)

watch(volumen, (newVal) => {
  if (props.audio?.value) props.audio.value.volume = newVal
  emit('update:volume', newVal)
})

</script>

<template>
  <div class="volumen">
    <input type="range" min="0" max="1" step="0.01" v-model="volumen" />
    <span>{{ Math.round(volumen*100) }}%</span>
  </div>
</template>

<style scoped>
.volumen {
  display: flex;
  align-items: center;
  gap: 10px;
}
input[type=range] { width: 150px; cursor: pointer; }
</style>
