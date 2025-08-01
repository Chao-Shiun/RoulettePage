<template>
  <div class="roulette-container">
    <div class="wheel-wrapper">
      <div class="arrow"></div>
      <svg
        class="wheel"
        :class="{ spinning: isSpinning }"
        :style="{ transform: `rotate(${rotation}deg)` }"
        width="400"
        height="400"
        viewBox="0 0 400 400"
      >
        <g v-for="(item, index) in items" :key="index">
          <path
            :d="getPath(index)"
            :fill="getColor(index)"
            stroke="white"
            stroke-width="2"
          />
          <text
            :x="getTextX(index)"
            :y="getTextY(index)"
            text-anchor="middle"
            dominant-baseline="middle"
            fill="white"
            font-size="14"
            font-weight="bold"
            :transform="`rotate(${getTextRotation(index)}, ${getTextX(index)}, ${getTextY(index)})`"
          >
            {{ item }}
          </text>
        </g>
      </svg>
    </div>
    <div class="controls">
      <textarea
        v-model="inputText"
        placeholder="輸入選項（每行一個）"
        rows="10"
        cols="30"
      ></textarea>
      <button @click="spin" :disabled="isSpinning || items.length === 0">
        {{ isSpinning ? '轉動中...' : '開始轉動' }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const inputText = ref('選項一\n選項二\n選項三\n選項四\n選項五')
const rotation = ref(0)
const isSpinning = ref(false)

const items = computed(() => {
  return inputText.value
    .split('\n')
    .filter(line => line.trim() !== '')
})

const colors = [
  '#FF6B6B', '#4ECDC4', '#45B7D1', '#FFA07A', '#98D8C8',
  '#F7DC6F', '#BB8FCE', '#85C1E2', '#F8C471', '#82E0AA'
]

const getColor = (index) => {
  return colors[index % colors.length]
}

const getPath = (index) => {
  const angle = 360 / items.value.length
  const startAngle = angle * index - 90
  const endAngle = startAngle + angle
  
  const startRad = (startAngle * Math.PI) / 180
  const endRad = (endAngle * Math.PI) / 180
  
  const x1 = 200 + 180 * Math.cos(startRad)
  const y1 = 200 + 180 * Math.sin(startRad)
  const x2 = 200 + 180 * Math.cos(endRad)
  const y2 = 200 + 180 * Math.sin(endRad)
  
  const largeArc = angle > 180 ? 1 : 0
  
  return `M 200 200 L ${x1} ${y1} A 180 180 0 ${largeArc} 1 ${x2} ${y2} Z`
}

const getTextX = (index) => {
  const angle = 360 / items.value.length
  const midAngle = angle * index + angle / 2 - 90
  const rad = (midAngle * Math.PI) / 180
  return 200 + 120 * Math.cos(rad)
}

const getTextY = (index) => {
  const angle = 360 / items.value.length
  const midAngle = angle * index + angle / 2 - 90
  const rad = (midAngle * Math.PI) / 180
  return 200 + 120 * Math.sin(rad)
}

const getTextRotation = (index) => {
  const angle = 360 / items.value.length
  return angle * index + angle / 2
}

const spin = () => {
  if (items.value.length === 0) return
  
  isSpinning.value = true
  const spins = 5 + Math.random() * 5
  const finalAngle = Math.random() * 360
  const totalRotation = rotation.value + spins * 360 + finalAngle
  
  rotation.value = totalRotation
  
  setTimeout(() => {
    isSpinning.value = false
    const normalizedAngle = totalRotation % 360
    const itemAngle = 360 / items.value.length
    const selectedIndex = Math.floor((360 - normalizedAngle) / itemAngle) % items.value.length
    alert(`恭喜！選中了：${items.value[selectedIndex]}`)
  }, 10000)
}
</script>

<style scoped>
.roulette-container {
  display: flex;
  gap: 40px;
  align-items: center;
  padding: 20px;
}

.wheel-wrapper {
  position: relative;
}

.arrow {
  position: absolute;
  top: -20px;
  left: 50%;
  transform: translateX(-50%);
  width: 0;
  height: 0;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
  border-top: 40px solid #FFD700;
  z-index: 10;
}

.wheel {
  display: block;
  transition: transform 10s cubic-bezier(0.17, 0.67, 0.12, 0.99);
}

.wheel.spinning {
  transition: transform 10s cubic-bezier(0.17, 0.67, 0.12, 0.99);
}

.controls {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

textarea {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  resize: vertical;
}

button {
  padding: 12px 24px;
  font-size: 16px;
  font-weight: bold;
  color: white;
  background-color: #4CAF50;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover:not(:disabled) {
  background-color: #45a049;
}

button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}
</style>