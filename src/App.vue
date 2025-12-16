<script setup>
import { ref } from 'vue'
import cancion from '@/assets/musica/Anemoia-LevitatingQuandary.mp3'
// import cancion from '@/assets/musica/.mp3'
// import cancion from '@/assets/musica/.mp3'
// import cancion from '@/assets/musica/.mp3'
// import cancion from '@/assets/musica/.mp3'

const audioplayer = ref(null)

const playSong = () => {
  audioplayer.value.play()
}

const pauseSong = () => {
  audioplayer.value.pause()
}

const fastForward = () => {
  if (audioplayer.value) 
  audioplayer.value.playbackRate = 4
}
const notFastForward = () => {
  if (audioplayer.value) 
  audioplayer.value.playbackRate = 1
}

const notBackForward = () => {
  if (audioplayer.value) 
  audioplayer.value.playbackRate = 1
}

const rewind = () => {
  if (audioplayer.value) {
    clearInterval(rewindInterval) // por si había uno activo
    rewindInterval = setInterval(() => {
      audioplayer.value.currentTime -= 0.2 // retrocede suavemente
      if (audioplayer.value.currentTime <= 0) {
        audioplayer.value.currentTime = 0
        clearInterval(rewindInterval)
      }
    }, 20)
  }
}

const stopRewind = () => {
  clearInterval(rewindInterval)
}

</script>


<template>
  <div class="contenedor">
    <audio ref="audioplayer" :src="cancion"></audio>

    <div id="botones">
      <button @click="playSong">▶️ Reproducir</button>
      <button @click="pauseSong">⏸️ Pausar</button>
      <button
        @mousedown="fastForward"
        @mouseup="notFastForward"
        @mouseleave="notBackForward"
      >⏩ Avanzar</button>
      <button 
        @mousedown="rewind"
        @mouseup="stopRewind"
        @mouseleave="stopRewind"
      >⏪ Retroceder</button>
    </div>
  </div>
</template>


<style scoped>
.contenedor {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
#botones button {
  margin-right: 5px;
}
</style>
