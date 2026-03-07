<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const active = ref(false)
const currentVerse = ref(-1) // -1 = intro, 0-6 = verses, 7 = chorus, 8 = outro
const showChorus = ref(false)
const exiting = ref(false)

const KONAMI = ['ArrowUp','ArrowUp','ArrowDown','ArrowDown','ArrowLeft','ArrowRight','ArrowLeft','ArrowRight','b','a']
let konamiIndex = 0
let timers = []

const cast = [
  {
    name: 'Noon',
    photo: 'https://www.noon-abdulqadir.com/authors/admin/avatar_hu_e880ef29dc9f459.jpg',
    color: '#C5E84C',
    verse: '♪ Half a million job ads and what did I find?\n"Energetic" means young, "mature" means declined! ♪',
    dance: 'dance-bounce',
  },
  {
    name: 'Fabio',
    photo: 'https://algosoc.org/uploads/_card/fabio-votta-1.jpg',
    color: '#1565C0',
    verse: '♪ They targeted your feed with your deepest fears —\nI scraped two million ads, took me several years! ♪',
    dance: 'dance-sway',
  },
  {
    name: 'Delaney',
    photo: 'https://hotpolitics.eu/wp-content/uploads/2025/12/Delaney_Peterson_2025-scaled-center-top-500x500.jpg',
    color: '#3E8EDE',
    verse: '♪ Your feelings are my variables, your rage my R² —\nPopulists sell belonging, returns never declared! ♪',
    dance: 'dance-wiggle',
  },
  {
    name: 'Roeland',
    photo: 'https://www.uva.nl/binaries/_ht_1747924741618/content/documents/personalpages/d/u/r.dubel/r.dubel',
    color: '#7CB9E8',
    verse: '♪ Do you trust the news? My data says you don\'t —\nI\'ll show you transparency, change your mind? I won\'t! ♪',
    dance: 'dance-bounce',
  },
  {
    name: 'Chiara',
    photo: 'https://cdn.bsky.app/img/avatar/plain/did:plc:squv6a7sg6vheny4xnanktgv/bafkreichklk5pv7lb6yll3r7vvtbltld6en52ur3v6huyyct3btyvxnrf4@jpeg',
    color: '#B8D8F0',
    verse: '♪ Parliament\'s a circus, insults are my data —\nCivility is dying, I\'ll write the autopsy later! ♪',
    dance: 'dance-sway',
  },
  {
    name: 'Ernesto',
    photo: 'https://www.ernesto-deleon.com/authors/admin/avatar_hu614f9917ce8b72d4443466d9e6bcba9e_221201_270x270_fill_q75_lanczos_center.jpg',
    color: '#C5E84C',
    verse: '♪ Your filter bubble\'s cozy but the truth is far away —\nI mapped the information, it gets lost along the way! ♪',
    dance: 'dance-wiggle',
  },
  {
    name: 'Jin',
    photo: 'https://media.licdn.com/dms/image/v2/D4E03AQHTIO8T-WxgDQ/profile-displayphoto-shrink_200_200/profile-displayphoto-shrink_200_200/0/1696669598795?e=2147483647&v=beta&t=qFivoYs98Ajqy1Sj1oBSFEdkJ6Koq3V169iQaS99mgs',
    color: '#3E8EDE',
    verse: '♪ The algorithm chose this song for your "For You" page —\nWho gets to feel empowered? Let me set the stage! ♪',
    dance: 'dance-bounce',
  },
]

const chorusText = '♪ We study screens and feeds and ads and trust and lies —\nCongratulations Dr. Noon, watch the bias fall! ♪'

function onKeyDown(e) {
  if (active.value) {
    if (e.key === 'Escape') endDance()
    return
  }
  if (e.key === KONAMI[konamiIndex]) {
    konamiIndex++
    if (konamiIndex === KONAMI.length) {
      konamiIndex = 0
      startDance()
    }
  } else {
    konamiIndex = e.key === KONAMI[0] ? 1 : 0
  }
}

function clearTimers() {
  timers.forEach(t => clearTimeout(t))
  timers = []
}

function startDance() {
  clearTimers()
  active.value = true
  exiting.value = false
  currentVerse.value = -1
  showChorus.value = false

  // Intro: all appear (1.5s)
  // Then each verse: 3.5s each (7 verses = 24.5s)
  // Then chorus: 5s
  // Then outro
  let delay = 1500

  for (let i = 0; i < cast.length; i++) {
    timers.push(setTimeout(() => { currentVerse.value = i }, delay))
    delay += 3500
  }

  // Chorus
  timers.push(setTimeout(() => {
    currentVerse.value = cast.length
    showChorus.value = true
  }, delay))

  // End
  timers.push(setTimeout(() => { endDance() }, delay + 5000))
}

function endDance() {
  clearTimers()
  exiting.value = true
  timers.push(setTimeout(() => {
    active.value = false
    exiting.value = false
    currentVerse.value = -1
    showChorus.value = false
  }, 800))
}

onMounted(() => {
  document.addEventListener('keydown', onKeyDown)
})

onUnmounted(() => {
  document.removeEventListener('keydown', onKeyDown)
  clearTimers()
})
</script>

<template>
  <Teleport to="body">
    <transition name="overlay">
      <div v-if="active" :class="['konami-overlay', { exiting }]" @click.self="endDance">
        <div class="stage-title" :class="{ visible: !showChorus }">
          🎵 The Dissertation Song 🎵
        </div>

        <div class="stage">
          <div
            v-for="(person, i) in cast"
            :key="person.name"
            :class="[
              'dancer',
              person.dance,
              { singing: currentVerse === i, 'all-dance': showChorus },
              currentVerse >= i ? 'entered' : '',
            ]"
            :style="{
              '--i': i,
              '--color': person.color,
              '--delay': (i * 0.12) + 's',
            }"
          >
            <transition name="verse-bubble">
              <div
                v-if="currentVerse === i"
                :class="['verse-bubble', i >= 5 ? 'bubble-left' : i <= 1 ? 'bubble-right' : '']"
              >
                {{ person.verse }}
              </div>
            </transition>
            <div class="dancer-avatar">
              <img :src="person.photo" :alt="person.name" />
            </div>
            <span class="dancer-name">{{ person.name }}</span>
            <div class="spotlight" />
          </div>
        </div>

        <transition name="chorus-fade">
          <div v-if="showChorus" class="chorus-banner">
            <div class="chorus-text">{{ chorusText }}</div>
            <div class="confetti-container">
              <span v-for="n in 40" :key="n" class="confetti-piece" :style="{ '--n': n }" />
            </div>
          </div>
        </transition>

        <div class="esc-hint">press ESC or click to exit</div>
      </div>
    </transition>
  </Teleport>
</template>

<style scoped>
.konami-overlay {
  position: fixed;
  inset: 0;
  z-index: 10000;
  background: radial-gradient(ellipse at center bottom, #0A2463 0%, #050e2d 70%, #020818 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  animation: overlayIn 0.6s ease forwards;
}

.konami-overlay.exiting {
  animation: overlayOut 0.8s ease forwards;
}

@keyframes overlayIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes overlayOut {
  from { opacity: 1; }
  to { opacity: 0; }
}

/* Stage title */
.stage-title {
  font-family: 'Playfair Display', serif;
  font-size: clamp(1.5rem, 4vw, 2.5rem);
  color: #C5E84C;
  text-align: center;
  margin-bottom: 2rem;
  opacity: 0;
  transform: scale(0.8);
  transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.stage-title.visible {
  opacity: 1;
  transform: scale(1);
}

/* Stage layout */
.stage {
  display: flex;
  gap: clamp(0.5rem, 2vw, 2rem);
  align-items: flex-end;
  justify-content: center;
  flex-wrap: wrap;
  padding: 0 1rem;
  position: relative;
  min-height: 280px;
}

/* Individual dancer */
.dancer {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  opacity: 0;
  transform: translateY(60px);
  transition: all 0.6s cubic-bezier(0.16, 1, 0.3, 1);
  transition-delay: var(--delay);
}

.dancer.entered {
  opacity: 1;
  transform: translateY(0);
}

.dancer-avatar {
  width: clamp(50px, 10vw, 80px);
  height: clamp(50px, 10vw, 80px);
  border-radius: 50%;
  overflow: hidden;
  border: 3px solid var(--color);
  box-shadow: 0 0 20px color-mix(in srgb, var(--color) 40%, transparent);
  transition: box-shadow 0.3s ease;
}

.dancer.singing .dancer-avatar {
  box-shadow:
    0 0 30px color-mix(in srgb, var(--color) 60%, transparent),
    0 0 60px color-mix(in srgb, var(--color) 30%, transparent);
}

.dancer-avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.dancer-name {
  margin-top: 0.5rem;
  font-size: 0.8rem;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.7);
  letter-spacing: 0.05em;
}

/* Spotlight under each dancer */
.spotlight {
  position: absolute;
  bottom: -30px;
  left: 50%;
  transform: translateX(-50%);
  width: 100px;
  height: 8px;
  background: radial-gradient(ellipse, color-mix(in srgb, var(--color) 30%, transparent), transparent);
  border-radius: 50%;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.dancer.singing .spotlight {
  opacity: 1;
}

/* Dance animations */
.dancer.singing.dance-bounce .dancer-avatar,
.dancer.all-dance.dance-bounce .dancer-avatar {
  animation: danceBounce 0.5s ease infinite;
}

.dancer.singing.dance-sway .dancer-avatar,
.dancer.all-dance.dance-sway .dancer-avatar {
  animation: danceSway 0.7s ease-in-out infinite;
}

.dancer.singing.dance-wiggle .dancer-avatar,
.dancer.all-dance.dance-wiggle .dancer-avatar {
  animation: danceWiggle 0.4s ease-in-out infinite;
}

/* When chorus, everyone dances regardless */
.dancer.all-dance {
  animation: chorusBounce 0.8s ease infinite;
  animation-delay: calc(var(--i) * 0.1s);
}

@keyframes danceBounce {
  0%, 100% { transform: translateY(0) scale(1); }
  50% { transform: translateY(-12px) scale(1.05); }
}

@keyframes danceSway {
  0%, 100% { transform: rotate(-6deg); }
  50% { transform: rotate(6deg); }
}

@keyframes danceWiggle {
  0%, 100% { transform: translateX(-4px) rotate(-3deg); }
  50% { transform: translateX(4px) rotate(3deg); }
}

@keyframes chorusBounce {
  0%, 100% { transform: translateY(0); }
  25% { transform: translateY(-8px) rotate(-3deg); }
  75% { transform: translateY(-8px) rotate(3deg); }
}

/* Verse bubble */
.verse-bubble {
  position: absolute;
  bottom: calc(100% + 16px);
  left: 50%;
  transform: translateX(-50%);
  background: rgba(255, 255, 255, 0.95);
  color: #0A2463;
  font-size: clamp(0.65rem, 1.4vw, 0.82rem);
  font-weight: 500;
  line-height: 1.5;
  padding: 0.75rem 1rem;
  border-radius: 14px;
  width: max-content;
  max-width: 260px;
  text-align: center;
  white-space: pre-line;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  pointer-events: none;
  z-index: 10;
}

.verse-bubble::after {
  content: '';
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border: 7px solid transparent;
  border-top-color: rgba(255, 255, 255, 0.95);
}

/* Shift bubble for edge dancers */
.verse-bubble.bubble-left {
  left: auto;
  right: 0;
  transform: none;
}

.verse-bubble.bubble-left::after {
  left: auto;
  right: 20px;
  transform: none;
}

.verse-bubble.bubble-right {
  left: 0;
  transform: none;
}

.verse-bubble.bubble-right::after {
  left: 20px;
  transform: none;
}

.verse-bubble-enter-active {
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.verse-bubble-leave-active {
  transition: all 0.25s ease-in;
}

.verse-bubble-enter-from {
  opacity: 0;
  transform: translateX(-50%) translateY(10px) scale(0.85);
}

.bubble-left.verse-bubble-enter-from,
.bubble-right.verse-bubble-enter-from {
  transform: translateY(10px) scale(0.85);
}

.verse-bubble-leave-to {
  opacity: 0;
  transform: translateX(-50%) translateY(-6px) scale(0.9);
}

.bubble-left.verse-bubble-leave-to,
.bubble-right.verse-bubble-leave-to {
  transform: translateY(-6px) scale(0.9);
}

/* Chorus banner */
.chorus-banner {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 20;
  pointer-events: none;
}

.chorus-text {
  background: linear-gradient(135deg, #C5E84C, #7CB9E8);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-family: 'Playfair Display', serif;
  font-size: clamp(1.1rem, 3vw, 1.8rem);
  font-weight: 700;
  text-align: center;
  line-height: 1.6;
  white-space: pre-line;
  padding: 2rem;
  animation: chorusPulse 1s ease-in-out infinite alternate;
}

@keyframes chorusPulse {
  from { transform: scale(1); filter: brightness(1); }
  to { transform: scale(1.04); filter: brightness(1.15); }
}

.chorus-fade-enter-active {
  transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.chorus-fade-leave-active {
  transition: all 0.4s ease-in;
}

.chorus-fade-enter-from,
.chorus-fade-leave-to {
  opacity: 0;
  transform: scale(0.8);
}

/* Confetti */
.confetti-container {
  position: fixed;
  inset: 0;
  pointer-events: none;
  overflow: hidden;
}

.confetti-piece {
  position: absolute;
  top: -10px;
  width: 8px;
  height: 8px;
  border-radius: 2px;
  opacity: 0;
  animation: confettiFall 3s ease-in forwards;
  animation-delay: calc(var(--n) * 0.08s);
}

.confetti-piece:nth-child(odd) {
  background: #C5E84C;
  left: calc(var(--n) * 2.5%);
}

.confetti-piece:nth-child(even) {
  background: #3E8EDE;
  left: calc(var(--n) * 2.5% + 1%);
}

.confetti-piece:nth-child(3n) {
  background: #7CB9E8;
  width: 6px;
  height: 10px;
  border-radius: 3px;
}

.confetti-piece:nth-child(5n) {
  background: #1565C0;
  width: 10px;
  height: 6px;
}

@keyframes confettiFall {
  0% {
    opacity: 1;
    transform: translateY(0) rotate(0deg);
  }
  100% {
    opacity: 0;
    transform: translateY(100vh) rotate(720deg);
  }
}

/* ESC hint */
.esc-hint {
  position: absolute;
  bottom: 1.5rem;
  color: rgba(255, 255, 255, 0.3);
  font-size: 0.75rem;
  letter-spacing: 0.1em;
}

/* Overlay transition */
.overlay-enter-active {
  transition: opacity 0.5s ease;
}

.overlay-leave-active {
  transition: opacity 0.5s ease;
}

.overlay-enter-from,
.overlay-leave-to {
  opacity: 0;
}

@media (max-width: 600px) {
  .stage {
    gap: 0.4rem;
  }

  .dancer-avatar {
    width: 44px;
    height: 44px;
  }

  .verse-bubble {
    max-width: 180px;
    font-size: 0.6rem;
    padding: 0.5rem 0.7rem;
  }

  .dancer-name {
    font-size: 0.65rem;
  }
}
</style>
