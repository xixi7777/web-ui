<template>
  <div class="wave-container p-20">
    <el-card header="SVG">
      <div class="wave svg">
        <svg width="200px" height="200px" version="1.1" xmlns="http://www.w3.org/2000/svg">
          <text class="liquidFillGaugeText" text-anchor="middle" font-size="42px" transform="translate(100,120)" style="fill: #000">50.0%</text>
          <!-- Wave -->
          <g id="wave">
              <path id="wave-2" fill="rgba(154, 205, 50, .8)" d="M 0 100 C 133.633 85.12 51.54 116.327 200 100 A 95 95 0 0 1 0 100 Z">
              <animate dur="5s" repeatCount="indefinite" attributeName="d" attributeType="XML" values="M0 100 C90 28, 92 179, 200 100 A95 95 0 0 1 0 100 Z;
                                          M0 100 C145 100, 41 100, 200 100 A95 95 0 0 1 0 100 Z;
                                          M0 100 C90 28, 92 179, 200 100 A95 95 0 0 1 0 100 Z"></animate>
              </path>
          </g>
          <circle cx="100" cy="100" r="80" stroke-width="10" stroke="white" fill="transparent"></circle>
          <circle cx="100" cy="100" r="90" stroke-width="20" stroke="yellowgreen" fill="none" class="percentage-pie-svg"></circle>
        </svg>
      </div>
    </el-card>
    <el-card header="Canvas" class="mt-20">
      <div class="wave canvas">
        <canvas id="canvas" width="200" height="200"></canvas>
      </div>
    </el-card>
    <el-card header="CSS" class="mt-20">
      <div class="wave css"></div>
    </el-card>
  </div>
</template>
<script setup lang="ts" name="wave">
import { onMounted } from 'vue';

onMounted(() => {
  init()
})

const init = () => {
  const canvas = document.querySelector('canvas') as HTMLCanvasElement
  const ctx = canvas.getContext('2d') as CanvasRenderingContext2D
  const radius = (Math.PI / 180) * 180
  let startTime = Date.now()
  let time = 2000
  let clockWise = true
  let [cp1x, cp1y, cp2x, cp2y] = [0, 0, 0, 0];

  requestAnimationFrame(function waveDraw() {
    let t = Math.min(1.0, (Date.now() - startTime) / time)

    if (clockWise) {
      cp1x = 90 + (55 * t)
      cp1y = 28 + (72 * t)
      cp2x = 92 - (51 * t)
      cp2y = 179 - (79 * t)
    } else {
      cp1x = 145 - (55 * t)
      cp1y = 100 - (72 * t)
      cp2x = 41 + (51 * t)
      cp2y = 100 + (79 * t)
    }

    ctx.clearRect(0, 0, 200, 200)
    ctx.beginPath()
    ctx.moveTo(0, 100)
    ctx.bezierCurveTo(cp1x, cp1y, cp2x, cp2y, 200, 100)
    ctx.arc(100, 100, 100, 0, radius, false)
    ctx.fillStyle = 'rgba(145, 205, 50, 0.8)'
    ctx.fill()
    ctx.save()

    if (t == 1) {
      startTime = Date.now()
      clockWise = !clockWise
    }

    requestAnimationFrame(waveDraw)
  })
}
</script>
<style lang="scss" scoped>
</style>