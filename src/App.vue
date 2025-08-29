<template>
  <div>
    <header>
      <div>SHOUKO</div>
    </header>

    <div class="slider">
      <div class="list" :style="{ transform: `translateX(${leftTransform}px)` }">
        <div
            v-for="(item, index) in items"
            :key="index"
            class="item"
            :class="{ active: active === index }"
        >
          <img :src="item" />
        </div>
      </div>

      <div class="circle" ref="circleRef">
        CLUB DAY - CHTCoder - funny moments, endless memories
      </div>

      <div class="content">
        <div>game</div>
        <div>club day</div>
        <button>Play</button>
      </div>

      <div class="arow">
        <button id="prev" @click="prevSlide"><</button>
        <button id="next" @click="nextSlide">></button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue'

const items = [
  '/img/1.png',
  '/img/2.png',
  '/img/3.png',
  '/img/4.png',
  '/img/5.png',
]

const active = ref(1)
const leftTransform = ref(0)
const widthItem = ref(0)
const circleRef = ref<HTMLDivElement | null>(null)

function runCarousel() {
  if (!widthItem.value) return
  leftTransform.value = widthItem.value * (active.value - 1) * -1
}

function nextSlide() {
  if (active.value < items.length - 1) {
    active.value++
    runCarousel()
  }
}

function prevSlide() {
  if (active.value > 0) {
    active.value--
    runCarousel()
  }
}

onMounted(async () => {
  await nextTick()
  const firstItem = document.querySelector<HTMLDivElement>('.list .item')
  if (firstItem) widthItem.value = firstItem.offsetWidth
  runCarousel()

  if (circleRef.value) {
    const textCircle = circleRef.value.innerText.split('')
    circleRef.value.innerText = ''

    textCircle.forEach((value, key) => {
      const newSpan = document.createElement('span')
      newSpan.innerText = value
      const rotateThisSpan = (360 / textCircle.length) * (key + 1)
      newSpan.style.setProperty('--rotate', `${rotateThisSpan}deg`)
      circleRef.value?.appendChild(newSpan)
    })
  }
})
</script>