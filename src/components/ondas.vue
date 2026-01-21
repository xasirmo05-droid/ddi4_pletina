<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  audio: Object,
  start: Boolean
})

const canvasRef = ref(null)
let audioContext, analyser, dataArray, source
let animating = false

watch(() => props.start, (val) => {
  if (val && !animating && props.audio?.value) startVisualizer()
})

const startVisualizer = async () => {
  if (!props.audio?.value) return

  if (!audioContext) {
    audioContext = new (window.AudioContext || window.webkitAudioContext)()
    await audioContext.resume()          // desbloquea el audio
    source = audioContext.createMediaElementSource(props.audio.value)
    analyser = audioContext.createAnalyser()
    source.connect(analyser)
    analyser.connect(audioContext.destination)
    analyser.fftSize = 256
    dataArray = new Uint8Array(analyser.frequencyBinCount)
  }

  animating = true
  draw()
}

const draw = () => {
  if (!canvasRef.value || !analyser || !dataArray) return

  requestAnimationFrame(draw)

  const canvas = canvasRef.value
  const ctx = canvas.getContext('2d')
  const width = canvas.width
  const height = canvas.height

  analyser.getByteFrequencyData(dataArray)

  ctx.fillStyle = '#111'
  ctx.fillRect(0, 0, width, height)

  const barWidth = width / dataArray.length
  for (let i = 0; i < dataArray.length; i++) {
    const barHeight = (dataArray[i] / 255) * height
    ctx.fillStyle = `hsl(${i/dataArray.length*360}, 80%, 50%)`
    ctx.fillRect(i * barWidth, height - barHeight, barWidth - 1, barHeight)
  }
}
</script>

<template>
  <canvas ref="canvasRef" width="500" height="150" style="border-radius:8px;"></canvas>
</template>

<style scoped>
canvas {
  display: block;
  background-color: #111;
}
</style>
