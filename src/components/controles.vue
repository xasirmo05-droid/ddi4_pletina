<script setup>
  import { defineEmits } from 'vue'

  const emit = defineEmits([
    'play',
    'pause',
    'fastForward',
    'normalSpeed',
    'rewind',
    'stopRewind',
    'cancionA',
    'cancionB'
  ])
</script>

<template>
  <div class="botones">
    <button
      @mousedown="emit('rewind')"
      @mouseup="emit('stopRewind')"
      @mouseleave="emit('stopRewind')">
      ⏪ Retroceder
    </button>

    <button @click="emit('play')">▶️ Reproducir</button>
    <button @click="emit('pause')">⏸️ Pausar</button>

    <button
      @mousedown="emit('fastForward')"
      @mouseup="emit('normalSpeed')"
      @mouseleave="emit('normalSpeed')">
      ⏩ Avanzar
    </button>
    <button @click="emit('cancionA')">🎵 Canción A</button>
    <button @click="emit('cancionB')">🎶 Canción B</button>
  </div>
</template>

<style scoped>
.botones {
  display: flex;
  gap: 10px;
}
button {
  padding: 6px 12px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  background-color: #eee;
  transition: 0.2s;
}
button:hover { background-color: #ddd; }
</style>
