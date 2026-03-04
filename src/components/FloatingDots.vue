<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const containerRef = ref(null)
const dotsConfig = ref([])

const colors = [
  '#0A2463', '#1565C0', '#3E8EDE', '#7CB9E8', '#B8D8F0',
  '#C5E84C', '#C5E84C', '#C5E84C', '#D4F06A', '#D4F06A',
  '#B8E035', '#EEFAB8',
]

// Physics (only active after drag/fling)
const PHYSICS_DAMPING = 0.993
const WALL_RESTITUTION = 0.25
const COLLISION_RESTITUTION = 0.3
const MAX_VELOCITY = 8
const SETTLE_SPEED = 0.2

let bodies = []
let rafId = null
let containerW = 0
let containerH = 0
let dragBody = null
let dragEl = null
let dragTargetX = 0
let dragTargetY = 0
let pointerHistory = []
let dotEls = []

function isInCoreText(xPct, yPct) {
  return xPct > 30 && xPct < 70 && yPct > 22 && yPct < 65
}

function init() {
  if (!containerRef.value) return
  const rect = containerRef.value.getBoundingClientRect()
  containerW = rect.width
  containerH = rect.height

  bodies = []
  const configs = []

  for (let i = 0; i < 80; i++) {
    let xPct = Math.random() * 100
    let yPct = Math.random() * 100
    let size = Math.random() * 60 + 10
    let opacity = Math.random() * 0.4 + 0.1

    if (isInCoreText(xPct, yPct)) {
      if (Math.random() > 0.5) {
        size = Math.random() * 25 + 6
        opacity = Math.random() * 0.12 + 0.04
      } else {
        const edge = Math.floor(Math.random() * 4)
        if (edge === 0) xPct = Math.random() * 28
        else if (edge === 1) xPct = 72 + Math.random() * 28
        else if (edge === 2) yPct = Math.random() * 20
        else yPct = 67 + Math.random() * 33
      }
    }

    const radius = size / 2
    const color = colors[Math.floor(Math.random() * colors.length)]
    const homeX = (xPct / 100) * containerW
    const homeY = (yPct / 100) * containerH

    bodies.push({
      x: homeX,
      y: homeY,
      vx: 0,
      vy: 0,
      radius,
      mass: radius * radius * 0.01 + 0.1,
      baseOpacity: opacity,
      dragging: false,
      mode: 'wander', // 'wander' or 'physics'
      // Smooth Lissajous wander parameters (unique per dot)
      homeX,
      homeY,
      wanderRx: 3 + Math.random() * 8,
      wanderRy: 2 + Math.random() * 6,
      wanderSpeedX: 0.00015 + Math.random() * 0.00025,
      wanderSpeedY: 0.0001 + Math.random() * 0.0002,
      wanderPhaseX: Math.random() * Math.PI * 2,
      wanderPhaseY: Math.random() * Math.PI * 2,
    })

    configs.push({ id: i, size, color })
  }

  dotsConfig.value = configs
}

function tick() {
  // Smooth drag: interpolate toward pointer instead of snapping
  if (dragBody) {
    dragBody.x += (dragTargetX - dragBody.x) * 0.35
    dragBody.y += (dragTargetY - dragBody.y) * 0.35
  }

  for (const b of bodies) {
    if (b.dragging) continue

    if (b.mode === 'wander') {
      // Smooth Lissajous wandering — dreamy, flowing paths
      b.wanderPhaseX += b.wanderSpeedX
      b.wanderPhaseY += b.wanderSpeedY
      const newX = b.homeX + Math.sin(b.wanderPhaseX) * b.wanderRx
      const newY = b.homeY + Math.sin(b.wanderPhaseY) * b.wanderRy
      b.vx = newX - b.x
      b.vy = newY - b.y
      b.x = newX
      b.y = newY
    } else {
      // Physics mode (after being flung)
      b.vx *= PHYSICS_DAMPING
      b.vy *= PHYSICS_DAMPING
      b.x += b.vx
      b.y += b.vy

      // Soft wall spring — smooth deceleration and push-back
      const wallForce = 0.05
      if (b.x - b.radius < 0) b.vx += (b.radius - b.x) * wallForce
      if (b.x + b.radius > containerW) b.vx -= (b.x + b.radius - containerW) * wallForce
      if (b.y - b.radius < 0) b.vy += (b.radius - b.y) * wallForce
      if (b.y + b.radius > containerH) b.vy -= (b.y + b.radius - containerH) * wallForce

      // Gentle clamp
      b.x = Math.max(-b.radius, Math.min(containerW + b.radius, b.x))
      b.y = Math.max(-b.radius, Math.min(containerH + b.radius, b.y))

      // Settle back to wander when slow enough
      const speed = Math.sqrt(b.vx * b.vx + b.vy * b.vy)
      if (speed < SETTLE_SPEED) {
        b.mode = 'wander'
        b.homeX = b.x
        b.homeY = b.y
        b.homeX = Math.max(b.radius + 10, Math.min(containerW - b.radius - 10, b.homeX))
        b.homeY = Math.max(b.radius + 10, Math.min(containerH - b.radius - 10, b.homeY))
        // Reset phase so sin(0)=0 → no teleport jump
        b.wanderPhaseX = 0
        b.wanderPhaseY = 0
        b.vx = 0
        b.vy = 0
      }
    }
  }

  // Collisions (only between physics-mode dots, or physics hitting wander)
  for (let i = 0; i < bodies.length; i++) {
    for (let j = i + 1; j < bodies.length; j++) {
      resolveCollision(bodies[i], bodies[j])
    }
  }

  render()
  rafId = requestAnimationFrame(tick)
}

function resolveCollision(a, b) {
  const dx = b.x - a.x
  const dy = b.y - a.y
  const distSq = dx * dx + dy * dy
  const minDist = a.radius + b.radius

  if (distSq >= minDist * minDist || distSq < 0.001) return

  const aPhysics = a.mode === 'physics' || a.dragging
  const bPhysics = b.mode === 'physics' || b.dragging

  // Both wandering: no collision at all
  if (!aPhysics && !bPhysics) return

  const dist = Math.sqrt(distSq)
  const nx = dx / dist
  const ny = dy / dist
  const overlap = minDist - dist

  // Pure spring force — inherently smooth, no instant jumps
  const force = overlap * 0.03
  if (!a.dragging) { a.vx -= nx * force; a.vy -= ny * force }
  if (!b.dragging) { b.vx += nx * force; b.vy += ny * force }
}

function render() {
  if (!dotEls.length && containerRef.value) {
    dotEls = Array.from(containerRef.value.querySelectorAll('.dot'))
  }
  for (let i = 0; i < bodies.length && i < dotEls.length; i++) {
    const b = bodies[i]
    dotEls[i].style.transform = `translate(${b.x - b.radius}px, ${b.y - b.radius}px)`
    dotEls[i].style.opacity = b.baseOpacity
  }
}

function onPointerDown(e, index) {
  e.preventDefault()
  e.stopPropagation()
  const b = bodies[index]
  if (!b) return

  b.dragging = true
  b.mode = 'physics'
  b.vx = 0
  b.vy = 0
  dragBody = b
  dragTargetX = b.x
  dragTargetY = b.y
  dragEl = e.currentTarget
  dragEl.classList.add('dragging')
  pointerHistory = [{ x: e.clientX, y: e.clientY, t: performance.now() }]

  document.addEventListener('pointermove', onPointerMove)
  document.addEventListener('pointerup', onPointerUp)
}

function onPointerMove(e) {
  if (!dragBody || !containerRef.value) return
  e.preventDefault()
  const rect = containerRef.value.getBoundingClientRect()
  dragTargetX = e.clientX - rect.left
  dragTargetY = e.clientY - rect.top

  pointerHistory.push({ x: e.clientX, y: e.clientY, t: performance.now() })
  if (pointerHistory.length > 5) pointerHistory.shift()
}

function onPointerUp() {
  if (dragBody) {
    dragBody.dragging = false
    if (pointerHistory.length >= 2) {
      const last = pointerHistory[pointerHistory.length - 1]
      const first = pointerHistory[0]
      const dt = (last.t - first.t) || 1
      dragBody.vx = (last.x - first.x) / dt * 6
      dragBody.vy = (last.y - first.y) / dt * 6
      const speed = Math.sqrt(dragBody.vx ** 2 + dragBody.vy ** 2)
      if (speed > MAX_VELOCITY) {
        dragBody.vx = (dragBody.vx / speed) * MAX_VELOCITY
        dragBody.vy = (dragBody.vy / speed) * MAX_VELOCITY
      }
    }
    dragBody = null
  }
  if (dragEl) {
    dragEl.classList.remove('dragging')
    dragEl = null
  }
  pointerHistory = []
  document.removeEventListener('pointermove', onPointerMove)
  document.removeEventListener('pointerup', onPointerUp)
}

function onResize() {
  if (!containerRef.value) return
  const rect = containerRef.value.getBoundingClientRect()
  const scaleX = rect.width / (containerW || rect.width)
  const scaleY = rect.height / (containerH || rect.height)
  containerW = rect.width
  containerH = rect.height
  for (const b of bodies) {
    b.x *= scaleX
    b.y *= scaleY
    b.homeX *= scaleX
    b.homeY *= scaleY
  }
}

onMounted(() => {
  init()
  rafId = requestAnimationFrame(tick)
  window.addEventListener('resize', onResize)
})

onUnmounted(() => {
  if (rafId) cancelAnimationFrame(rafId)
  window.removeEventListener('resize', onResize)
  document.removeEventListener('pointermove', onPointerMove)
  document.removeEventListener('pointerup', onPointerUp)
})
</script>

<template>
  <div ref="containerRef" class="floating-dots" aria-hidden="true">
    <div
      v-for="(dot, i) in dotsConfig"
      :key="dot.id"
      class="dot"
      :style="{
        width: dot.size + 'px',
        height: dot.size + 'px',
        backgroundColor: dot.color,
      }"
      @pointerdown="onPointerDown($event, i)"
    />
  </div>
</template>

<style scoped>
.floating-dots {
  position: absolute;
  inset: 0;
  overflow: hidden;
  pointer-events: none;
}

.dot {
  position: absolute;
  left: 0;
  top: 0;
  border-radius: 50%;
  pointer-events: auto;
  cursor: grab;
  touch-action: none;
  will-change: transform;
  transition: filter 0.15s ease;
}

.dot:hover {
  filter: brightness(1.1);
}

.dot.dragging {
  cursor: grabbing;
  z-index: 10;
  filter: brightness(1.2) drop-shadow(0 4px 8px rgba(0,0,0,0.15));
}
</style>
