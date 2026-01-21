<script setup>
import { ref, nextTick } from 'vue'
import Volumen from '@/components/volumen.vue'
import Controles from '@/components/controles.vue'
import Imagen from '@/components/imagen.vue'
import Ondas from '@/components/ondas.vue'

import cancionA from '@/assets/musica/Anemoia-LevitatingQuandary.mp3'
import cancionB from '@/assets/musica/weii.mp3'

const audioplayer = ref(null)
const volumenActual = ref(1)
const visualizadorActivo = ref(false)

const canciones = [cancionA, cancionB]
const indiceCancion = ref(0)
const cancion = ref(canciones[indiceCancion.value])

const reproducirCancion = async () => {
  if (!audioplayer.value) return

  visualizadorActivo.value = false
  audioplayer.value.pause()
  audioplayer.value.currentTime = 0

  cancion.value = canciones[indiceCancion.value]
  await nextTick()

  audioplayer.value.play()
  visualizadorActivo.value = true
}

const siguienteCancion = () => {
  indiceCancion.value =
    (indiceCancion.value + 1) % canciones.length
  reproducirCancion()
}

const anteriorCancion = () => {
  indiceCancion.value =
    (indiceCancion.value - 1 + canciones.length) % canciones.length
  reproducirCancion()
}

const handlePlay = () => {
  audioplayer.value?.play()
  visualizadorActivo.value = true
}

const handlePause = () => {
  audioplayer.value?.pause()
  visualizadorActivo.value = false
}

const handleVolumeChange = (val) => {
  volumenActual.value = val
  if (audioplayer.value) audioplayer.value.volume = val
}
</script>

<template>
  <div class="reproductor">
    <audio ref="audioplayer" :src="cancion"></audio>
    <Ondas :audio="audioplayer" :start="visualizadorActivo" />
    <Volumen :volume="volumenActual" @update:volume="handleVolumeChange" />
    <audio ref="audioplayer" :src="cancion"></audio>
    <Controles
      @play="handlePlay"
      @pause="handlePause"
      @prev="anteriorCancion"
      @next="siguienteCancion"
    />
    <Imagen :audio="audioplayer" :start="visualizadorActivo" />
  </div>
</template>
