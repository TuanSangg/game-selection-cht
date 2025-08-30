<template>
  <div>
    <header>
      <div class="club">
        <div>CHTCoder</div>
      </div>

      <a href="https://github.com/vedokoush/game-selection-cht" target="_blank" rel="noopener" class="github-link">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="github-icon">
          <path
              fill="currentColor"
              d="M12 0C5.37 0 0 5.37 0 12c0 5.3 3.438 9.8 8.205 11.385.6.113.82-.263.82-.582
                    0-.288-.01-1.05-.015-2.06-3.338.725-4.042-1.61-4.042-1.61-.546-1.385-1.333-1.754-1.333-1.754-1.09-.745.083-.73.083-.73
                    1.205.083 1.84 1.236 1.84 1.236 1.07 1.835 2.807 1.305 3.492.997.108-.775.418-1.305.762-1.605-2.665-.305-5.466-1.332-5.466-5.93
                    0-1.31.468-2.383 1.236-3.223-.123-.303-.54-1.527.117-3.176 0 0 1.008-.322 3.3 1.23a11.5 11.5 0 0 1 3.003-.404
                    11.5 11.5 0 0 1 3.003.404c2.292-1.552 3.3-1.23 3.3-1.23.657 1.649.24 2.873.117 3.176.768.84
                    1.236 1.913 1.236 3.223 0 4.61-2.803 5.624-5.475 5.922.43.37.823 1.096.823 2.21
                    0 1.595-.015 2.88-.015 3.267 0 .322.217.7.825.58C20.565 21.796 24 17.297 24 12
                    24 5.37 18.63 0 12 0z"
          />
        </svg>
      </a>
    </header>

    <div class="slider">
      <div class="list" :style="{ transform: `translateX(${leftTransform}px)` }">
        <div
            v-for="(item, index) in items"
            :key="index"
            class="item"
            :class="{ active: active === index }"
        >
          <img :src="item.img" />
          <div
              class="label"
              :style="{
      background: 'rgba(0,0,0,0.35)',
      color: item.color,
      textShadow: `0 0 10px ${item.color}, 0 0 20px ${item.color}80, 0 0 30px ${item.color}60`,
      border: `2px solid ${item.color}aa`
    }"
          >
            {{ item.name }}
          </div>
        </div>
      </div>

      <div class="circle" ref="circleRef">
        CLUB DAY - CHTCoder - funny moments, endless memories
      </div>

      <div class="content">
        <div>game</div>
        <div>club day</div>
        <button @click="goToLink">Play</button>
      </div>

      <div class="arow">
        <button id="prev" @click="prevSlide"><</button>
        <button id="next" @click="nextSlide">></button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

const items = [
  { img: '/img/math.PNG', link: 'https://youtu.be/xvFZjo5PgG0', name: 'Math', color: '#f2a6a5' },
  { img: '/img/lit.PNG', link: 'https://youtu.be/xvFZjo5PgG0', name: 'Literature', color: '#f0ce8a' },
  { img: '/img/it.PNG', link: 'https://youtu.be/xvFZjo5PgG0', name: 'IT', color: '#00bfff' },
  { img: '/img/geo.PNG', link: 'https://youtu.be/xvFZjo5PgG0', name: 'Geography', color: '#92c07a' },
  { img: '/img/physics.PNG', link: 'https://youtu.be/xvFZjo5PgG0', name: 'Physics', color: '#edd07b' },
  { img: '/img/chemistry.PNG', link: 'https://youtu.be/xvFZjo5PgG0', name: 'Chemistry', color: '#ffffff' },
]

const active = ref(1)
const leftTransform = ref(0)
const widthItem = ref(0)
const circleRef = ref<HTMLDivElement | null>(null)

function goToLink() {
  const link = items[active.value].link
  if (link) {
    window.open(link, '_blank')
  }
}

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

function updateWidth() {
  const firstItem = document.querySelector<HTMLDivElement>('.list .item')
  if (firstItem) {
    widthItem.value = firstItem.offsetWidth
  }
  runCarousel()
}

onMounted(async () => {
  await nextTick()
  updateWidth()
  window.addEventListener('resize', updateWidth)

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

onUnmounted(() => {
  window.removeEventListener('resize', updateWidth)
})
</script>
