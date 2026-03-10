<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import FloatingDots from './FloatingDots.vue'

const visible = ref(false)
const showDots = ref(false)
const revealFaces = ref(false)
const orbitRef = ref(null)

const noonQuotes = [
  '"Dynamic team player wanted" = we want a young woman. I have the data.',
  'I read 500,000 job ads so you don\'t have to. Spoiler: they\'re biased.',
  'Your job ad said "energetic." I know exactly what that means.',
  'Gender stereotypes in hiring? Let me show you the receipts.',
  'About to be Dr. Noon. The algorithm didn\'t see that coming.',
  '"Flexible and adaptable"? That\'s code. I cracked it.',
  'HR thinks job ads are neutral. My dissertation disagrees.',
  'Every adjective in your job ad is a political statement.',
  '"Must be a self-starter" — translation: we won\'t train you.',
  'I turned job ads into a dataset. It got uncomfortable fast.',
  '"Culture fit" is doing a lot of heavy lifting in that listing.',
  'Your recruiter has a type. I quantified it.',
  'The job ad said "mature." My regression said "ageist."',
  '"Digital native wanted." That\'s not a skill, that\'s a birth year.',
  'I read between the lines of job ads. It\'s not pretty.',
  'Age 55? The algorithm already archived your application.',
  '"Ambitious go-getter" wanted. Translation: be young and cheap.',
  'If your job ad were a person, it would fail a bias audit.',
  'I made recruiters read their own ads back. They were shook.',
  'The digital society hires like it\'s 1955. I have proof.',
]

const faces = [
  { name: 'Fabio', photo: 'https://algosoc.org/uploads/_card/fabio-votta-1.jpg', dot: '#1565C0', quotes: [
    'I know who targeted that ad at you.',
    'Your feed? Not random. Never was.',
    'Political microtargeting is my cardio.',
    'The algorithm knows your politics.',
    'Campaign data doesn\'t lie. Politicians do.',
    'That sponsored post? I know who paid.',
    'Facebook knows your vote. I proved it.',
    'Dark ads aren\'t dark if I can see them.',
    'Your political identity is a product.',
    'Targeted by a party? I have the receipt.',
    'Ad transparency is my love language.',
    'I scraped 2 million political ads. For fun.',
    'You clicked that ad. They know everything.',
    'Microtargeting: it\'s personal. Literally.',
    'Who paid for that ad? I already know.',
    'Platforms are the new campaign managers.',
    'Your swing vote has a price tag. I\'ve seen it.',
    'I make political ads confess their secrets.',
    'Data-driven democracy? More like data-driven manipulation.',
    'I audit ads so democracy has a chance.',
  ]},
  { name: 'Delaney', photo: 'https://hotpolitics.eu/wp-content/uploads/2025/12/Delaney_Peterson_2025-scaled-center-top-500x500.jpg', dot: '#C5E84C', quotes: [
    'Lonely? A populist has entered the chat.',
    'Your mood predicts your vote. Sorry.',
    'Hot take: feelings win elections.',
    'Doom-scrolling is my research method.',
    'Politics is just therapy with worse outcomes.',
    'Angry tweets predict election outcomes.',
    'Populism thrives on loneliness. So does Twitter.',
    'Your emotions are a political variable.',
    'Rage-clicking? That\'s a voter behavior.',
    'I measure vibes. Academically.',
    'Every doomscroll is a data point.',
    'Emotional contagion? Also known as TikTok.',
    'Feelings don\'t care about your facts either.',
    'Your anxiety has electoral consequences.',
    'Populists sell belonging. Returns not accepted.',
    'I regress emotions on vote choice. It works.',
    'Your political rage? Statistically significant.',
    'Lonely voters, loud populists. I see the pattern.',
    'Affective polarization is my comfort zone.',
    'Your gut feeling is someone\'s campaign strategy.',
  ]},
  { name: 'Roeland', photo: 'https://www.uva.nl/binaries/_ht_1747924741618/content/documents/personalpages/d/u/r.dubel/r.dubel', dot: '#7CB9E8', quotes: [
    'Do you trust this speech bubble?',
    'Transparency ≠ trust. Plot twist.',
    '"Fake news" — my whole PhD in two words.',
    'I study trust. Ironically, trust me.',
    'Journalists try. Readers say "meh."',
    'Fact-check this: trust is complicated.',
    'More transparency, less trust. Weird, right?',
    'Your news diet is my research question.',
    'Media literacy: necessary but insufficient.',
    '"I did my own research" — famous last words.',
    'The trust paradox is not a band name.',
    'Credibility is earned. Then immediately lost.',
    'You trust strangers on Reddit more than the BBC.',
    'I study why you don\'t believe the news.',
    'Healthy skepticism has left the chat.',
    'Your distrust is my dependent variable.',
    'News avoidance: the new binge-watching.',
    '"According to sources" — but which ones?',
    'If trust were easy, I\'d be unemployed.',
    'I peer-review your news consumption habits.',
  ]},
  { name: 'Chiara', photo: 'https://cdn.bsky.app/img/avatar/plain/did:plc:squv6a7sg6vheny4xnanktgv/bafkreichklk5pv7lb6yll3r7vvtbltld6en52ur3v6huyyct3btyvxnrf4@jpeg', dot: '#B8D8F0', quotes: [
    'Politicians being rude? That\'s my dataset.',
    'I measure trash-talk for a living.',
    'Your hot take? I have a codebook for that.',
    'Incivility is rising. My N is thriving.',
    'Democracy: now with more insults.',
    'Parliamentary debate or WWE? Hard to tell.',
    'I code insults for science.',
    'Civility is dead. I\'m writing the autopsy.',
    'Your comment section is my lab.',
    'Political discourse? More like political roast.',
    'Trolls are just unpaid research assistants.',
    'Name-calling in parliament: N = too many.',
    'I have a PhD in people being mean online.',
    'Rudeness as rhetoric: surprisingly effective.',
    'Your outrage is peer-reviewed.',
    'Deliberative democracy? Have you seen Twitter?',
    'I measure toxicity. It\'s thriving.',
    'Every insult is a data point. Lucky me.',
    'Norm erosion: when "you\'re wrong" becomes "you\'re evil."',
    'Political manners went extinct. I have the data.',
  ]},
  { name: 'Ernesto', photo: 'https://www.ernesto-deleon.com/authors/admin/avatar_hu614f9917ce8b72d4443466d9e6bcba9e_221201_270x270_fill_q75_lanczos_center.jpg', dot: '#D4F06A', quotes: [
    'Your filter bubble is showing.',
    'News takes weird detours online.',
    'Shared reality? In this economy?',
    'I study info flows. Yours is mostly memes.',
    'The algorithm curates. I investigate.',
    'Echo chambers have great acoustics.',
    'Information doesn\'t want to be free. It wants to go viral.',
    'Your media diet needs more fiber.',
    'I mapped your news ecosystem. It\'s a mess.',
    'The algorithm picks your reality. Daily.',
    'Cross-cutting exposure is my white whale.',
    'Your news bubble? Surprisingly cozy.',
    'Fragmentation: when everyone has their own truth.',
    'I follow the memes to find the news.',
    'Selective exposure: academic term for "confirmation bias on steroids."',
    'Your timeline is a funhouse mirror of reality.',
    'I track how facts become opinions become memes.',
    'Information inequality is the new digital divide.',
    'You and your neighbor see different internets.',
    'I study what you don\'t see. And you don\'t see a lot.',
  ]},
  { name: 'Jin', photo: 'https://media.licdn.com/dms/image/v2/D4E03AQHTIO8T-WxgDQ/profile-displayphoto-shrink_200_200/profile-displayphoto-shrink_200_200/0/1696669598795?e=2147483647&v=beta&t=qFivoYs98Ajqy1Sj1oBSFEdkJ6Koq3V169iQaS99mgs', dot: '#3E8EDE', quotes: [
    'The algorithm decides if you care about politics.',
    'Your recommender system is gaslighting you.',
    'Political efficacy? The algorithm ate it.',
    'Do you feel empowered? Let me check your feed.',
    'Algorithmic gatekeeping: it\'s not you, it\'s the code.',
    'Your sense of political power? Algorithmically curated.',
    'Recommendations shape democracies. No pressure.',
    'The feed giveth, the feed taketh away.',
    'You think you chose that video. You didn\'t.',
    'AI doesn\'t vote. But it decides who does.',
    'Your "For You" page is a political institution.',
    'Algorithmic amplification is the new gerrymandering.',
    'I study invisible editorial decisions.',
    'The algorithm is the editor-in-chief now.',
    'Auto-play is a democratic experiment.',
    'Your engagement is their business model.',
    'I decode what the algorithm hides from you.',
    'Political participation? There\'s an algorithm for that.',
    'The code doesn\'t care about democracy. I do.',
    'Your next click was already decided for you.',
  ]},
]

const hoveredFace = ref(-1)
const faceQuoteIdx = ref({})
const noonHovered = ref(false)
const noonQuoteIdx = ref(0)

function onFaceHover(i) {
  const quotes = faces[i].quotes
  let idx = Math.floor(Math.random() * quotes.length)
  if (faceQuoteIdx.value[i] === idx) idx = (idx + 1) % quotes.length
  faceQuoteIdx.value[i] = idx
  hoveredFace.value = i
}

function onFaceLeave() {
  hoveredFace.value = -1
}

function onNoonHover() {
  let idx = Math.floor(Math.random() * noonQuotes.length)
  if (noonQuoteIdx.value === idx) idx = (idx + 1) % noonQuotes.length
  noonQuoteIdx.value = idx
  noonHovered.value = true
}

function onNoonLeave() {
  noonHovered.value = false
}

let timerId = null
let revealTimer = null
let rafId = null

// Physics state for each face bubble
const FACE_SPRING = 0.003
const FACE_DAMPING = 0.96
const FACE_RESTITUTION = 0.7
const FACE_RADIUS = 19
const FACE_MAX_V = 25
const facePhysics = Array.from({ length: 6 }, () => ({
  x: 0, y: 0, vx: 0, vy: 0,
  physicsActive: false, dragging: false,
}))

function updateFaces(rotation, radius) {
  if (!orbitRef.value) return
  const slots = orbitRef.value.querySelectorAll('.face-slot')

  // Step 1: compute orbit targets & update physics
  slots.forEach((slot, i) => {
    const fp = facePhysics[i]
    const angle = (i * 60 + rotation) * Math.PI / 180
    const targetX = Math.cos(angle) * radius
    const targetY = Math.sin(angle) * radius

    if (fp.dragging) return

    if (fp.physicsActive) {
      // Spring force toward orbit target
      fp.vx += (targetX - fp.x) * FACE_SPRING
      fp.vy += (targetY - fp.y) * FACE_SPRING
      fp.vx *= FACE_DAMPING
      fp.vy *= FACE_DAMPING
      fp.x += fp.vx
      fp.y += fp.vy

      // Settle check
      const dist = Math.sqrt((targetX - fp.x) ** 2 + (targetY - fp.y) ** 2)
      const speed = Math.sqrt(fp.vx ** 2 + fp.vy ** 2)
      if (dist < 2 && speed < 0.1) {
        fp.physicsActive = false
      }
    } else {
      fp.x = targetX
      fp.y = targetY
    }
  })

  // Step 2: face-face collisions
  for (let i = 0; i < facePhysics.length; i++) {
    for (let j = i + 1; j < facePhysics.length; j++) {
      resolveFaceCollision(facePhysics[i], facePhysics[j])
    }
  }

  // Step 3: render
  slots.forEach((slot, i) => {
    const fp = facePhysics[i]
    if (!fp.dragging) {
      slot.style.transform = `translate(${fp.x}px, ${fp.y}px)`
    }
  })
}

function resolveFaceCollision(a, b) {
  if (!a.physicsActive && !b.physicsActive) return
  const dx = b.x - a.x
  const dy = b.y - a.y
  const distSq = dx * dx + dy * dy
  const minDist = FACE_RADIUS * 2
  if (distSq >= minDist * minDist || distSq < 0.001) return

  const dist = Math.sqrt(distSq)
  const nx = dx / dist
  const ny = dy / dist
  const overlap = minDist - dist

  // Separate
  const aMove = a.physicsActive && !a.dragging
  const bMove = b.physicsActive && !b.dragging
  if (aMove && bMove) {
    a.x -= nx * overlap * 0.5; a.y -= ny * overlap * 0.5
    b.x += nx * overlap * 0.5; b.y += ny * overlap * 0.5
  } else if (aMove) {
    a.x -= nx * overlap; a.y -= ny * overlap
  } else if (bMove) {
    b.x += nx * overlap; b.y += ny * overlap
  }

  // Impulse
  const dvn = (a.vx - b.vx) * nx + (a.vy - b.vy) * ny
  if (dvn < 0) return
  const impulse = dvn * FACE_RESTITUTION

  if (aMove) { a.vx -= impulse * nx; a.vy -= impulse * ny }
  if (bMove || !b.physicsActive) {
    b.vx += impulse * nx; b.vy += impulse * ny
    b.physicsActive = true // wake up on impact
  }
}

function onFacePointerDown(e, index) {
  e.preventDefault()
  e.stopPropagation()
  const slot = e.currentTarget
  const track = orbitRef.value
  if (!track) return
  const trackRect = track.getBoundingClientRect()
  const centerX = trackRect.left + trackRect.width / 2
  const centerY = trackRect.top + trackRect.height / 2

  const fp = facePhysics[index]
  fp.dragging = true
  fp.physicsActive = true
  fp.vx = 0
  fp.vy = 0
  slot.classList.add('face-dragging')

  let pHistory = [{ x: e.clientX, y: e.clientY, t: performance.now() }]

  function onMove(ev) {
    ev.preventDefault()
    fp.x = ev.clientX - centerX
    fp.y = ev.clientY - centerY
    slot.style.transform = `translate(${fp.x}px, ${fp.y}px)`
    pHistory.push({ x: ev.clientX, y: ev.clientY, t: performance.now() })
    if (pHistory.length > 5) pHistory.shift()
  }

  function onUp() {
    fp.dragging = false
    slot.classList.remove('face-dragging')

    if (pHistory.length >= 2) {
      const last = pHistory[pHistory.length - 1]
      const first = pHistory[0]
      const dt = (last.t - first.t) || 1
      fp.vx = (last.x - first.x) / dt * 16
      fp.vy = (last.y - first.y) / dt * 16
      const speed = Math.sqrt(fp.vx ** 2 + fp.vy ** 2)
      if (speed > FACE_MAX_V) {
        fp.vx = (fp.vx / speed) * FACE_MAX_V
        fp.vy = (fp.vy / speed) * FACE_MAX_V
      }
    }

    document.removeEventListener('pointermove', onMove)
    document.removeEventListener('pointerup', onUp)
  }

  document.addEventListener('pointermove', onMove)
  document.addEventListener('pointerup', onUp)
}

function startSpiral() {
  showDots.value = true

  // Phase 2: after 5s of being dots, start revealing faces
  revealTimer = setTimeout(() => { revealFaces.value = true }, 5000)

  const startTime = performance.now()
  const spiralDuration = 30000 // 30 seconds - very slow drift
  const startRadius = 150
  const endRadius = 78
  const spiralRotation = 270 // less than 1 full turn - gentle drift

  function animate(now) {
    const elapsed = now - startTime
    const progress = Math.min(elapsed / spiralDuration, 1)
    // Very smooth ease-out
    const eased = 1 - Math.pow(1 - progress, 2.5)

    const radius = startRadius - (startRadius - endRadius) * eased
    const rotation = spiralRotation * eased

    updateFaces(rotation, radius)

    if (progress < 1) {
      rafId = requestAnimationFrame(animate)
    } else {
      startGentleOrbit(endRadius, spiralRotation)
    }
  }

  rafId = requestAnimationFrame(animate)
}

function startGentleOrbit(radius, offsetRotation) {
  const startTime = performance.now()

  function animate(now) {
    const elapsed = now - startTime
    // 1 rotation per 60 seconds - barely noticeable
    const rotation = offsetRotation + (elapsed / 60000) * 360
    const breathe = Math.sin(elapsed / 3000) * 2
    updateFaces(rotation, radius + breathe)
    rafId = requestAnimationFrame(animate)
  }

  rafId = requestAnimationFrame(animate)
}

onMounted(() => {
  setTimeout(() => { visible.value = true }, 100)
  timerId = setTimeout(startSpiral, 30000)
})

onUnmounted(() => {
  if (timerId) clearTimeout(timerId)
  if (revealTimer) clearTimeout(revealTimer)
  if (rafId) cancelAnimationFrame(rafId)
})

function downloadCalendar() {
  const ics = [
    'BEGIN:VCALENDAR',
    'VERSION:2.0',
    'PRODID:-//Noon Defense//EN',
    'BEGIN:VEVENT',
    'DTSTART:20260409T080000Z',
    'DTEND:20260409T100000Z',
    'SUMMARY:Noon\'s Doctoral Defense - Hiring in the Digital Society',
    'DESCRIPTION:Public defense of the dissertation "Hiring in the Digital Society" by Noon M.F. Abdulqadir',
    'LOCATION:Agnietenkapel\\, Oudezijds Voorburgwal 229\\, 1012 EZ Amsterdam',
    'END:VEVENT',
    'BEGIN:VEVENT',
    'DTSTART:20260409T163000Z',
    'DTEND:20260409T210000Z',
    'SUMMARY:Noon\'s Defense Party at Cafe de Jaren',
    'DESCRIPTION:Celebration party for Dr. Noon M.F. Abdulqadir',
    'LOCATION:Cafe de Jaren\\, Nieuwe Doelenstraat 20\\, 1012 CP Amsterdam',
    'END:VEVENT',
    'END:VCALENDAR',
  ].join('\r\n')

  const blob = new Blob([ics], { type: 'text/calendar;charset=utf-8' })
  const url = URL.createObjectURL(blob)
  const a = document.createElement('a')
  a.href = url
  a.download = 'noon-defense-2026.ics'
  a.click()
  URL.revokeObjectURL(url)
}
</script>

<template>
  <section class="hero" id="hero">
    <FloatingDots />
    <div :class="['hero-content', { visible }]">
      <p class="hero-label">Doctoral Defense Ceremony</p>
      <h1 class="hero-title">
        <span class="title-line">Hiring in the</span>
        <span class="title-line accent">Digital Society</span>
      </h1>
      <p class="hero-subtitle">
        Content and Consequences of Gender and Age Stereotypes<br>in Job Advertisements
      </p>
      <div class="hero-author">
        <div class="orbit-system">
          <div
            class="author-photo-wrap"
            :class="{ 'faces-active': revealFaces }"
            @mouseenter="onNoonHover"
            @mouseleave="onNoonLeave"
          >
            <img
              src="https://www.noon-abdulqadir.com/authors/admin/avatar_hu_e880ef29dc9f459.jpg"
              alt="Noon M.F. Abdulqadir"
              class="author-photo"
            />
          </div>
          <transition name="bubble">
            <div v-if="noonHovered" class="hero-speech-bubble noon-bubble">
              {{ noonQuotes[noonQuoteIdx] }}
            </div>
          </transition>
          <div ref="orbitRef" :class="['orbit-track', { active: showDots, revealed: revealFaces }]">
            <div
              v-for="(face, i) in faces"
              :key="i"
              class="face-slot"
              :style="{ background: face.dot }"
              @pointerdown="onFacePointerDown($event, i)"
              @mouseenter="onFaceHover(i)"
              @mouseleave="onFaceLeave"
            >
              <img :src="face.photo" :alt="face.name" class="face-img" draggable="false" />
              <transition name="bubble">
                <div v-if="hoveredFace === i" class="hero-speech-bubble face-bubble">
                  {{ face.quotes[faceQuoteIdx[i] ?? 0] }}
                </div>
              </transition>
            </div>
          </div>
        </div>
        <span class="by">by</span>
        <span class="name">Noon M.F. Abdulqadir</span>
      </div>
      <div class="hero-meta">
        <div class="meta-item">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>
          <span>Thursday, 9 April 2026</span>
        </div>
        <div class="meta-item">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
          <span>10:00 AM</span>
        </div>
        <div class="meta-item">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>
          <span>Agnietenkapel, Amsterdam</span>
        </div>
      </div>
      <div class="hero-buttons">
        <a href="https://forms.gle/55D3ma1nGCNHivS88" target="_blank" rel="noopener" class="hero-cta rsvp">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
          RSVP Now
        </a>
        <button class="hero-cta calendar" @click="downloadCalendar">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>
          Add to Calendar
        </button>
        <a href="#schedule" class="hero-cta secondary">
          View Schedule
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="12" y1="5" x2="12" y2="19"/><polyline points="19 12 12 19 5 12"/></svg>
        </a>
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  background: linear-gradient(135deg, var(--white) 0%, var(--off-white) 50%, rgba(197, 232, 76, 0.05) 100%);
}

.hero-content {
  position: relative;
  z-index: 2;
  text-align: center;
  padding: 2rem;
  max-width: 800px;
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.hero-content.visible {
  opacity: 1;
  transform: translateY(0);
}

.hero-label {
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--blue);
  margin-bottom: 1.5rem;
}

.hero-title {
  font-size: clamp(2.5rem, 6vw, 4.5rem);
  font-weight: 700;
  color: var(--navy);
  margin-bottom: 1rem;
  line-height: 1.1;
}

.title-line {
  display: block;
}

.title-line.accent {
  color: var(--blue);
  font-style: italic;
}

.hero-subtitle {
  font-size: clamp(1rem, 2vw, 1.25rem);
  color: var(--blue-mid);
  font-weight: 400;
  max-width: 600px;
  margin: 0 auto 2rem;
  line-height: 1.5;
  font-style: italic;
  font-family: 'Playfair Display', serif;
}

.hero-author {
  margin-bottom: 2.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* === Orbit System === */
.orbit-system {
  position: relative;
  width: 120px;
  height: 120px;
  margin-bottom: 1rem;
}

.author-photo-wrap {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  overflow: hidden;
  box-shadow: 0 8px 30px rgba(10, 36, 99, 0.12);
  border: 4px solid var(--white);
  transition: all 2s ease;
  position: relative;
  z-index: 2;
}

.author-photo-wrap.faces-active {
  border-color: rgba(255, 255, 255, 0.4);
}

.author-photo-wrap:hover {
  transform: scale(1.05);
  box-shadow: 0 12px 40px rgba(10, 36, 99, 0.18);
}

.author-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.orbit-track {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  pointer-events: none;
  z-index: 3;
}

.orbit-track.revealed .face-slot {
  pointer-events: auto;
  cursor: grab;
  touch-action: none;
}

.face-slot.face-dragging {
  cursor: grabbing !important;
  z-index: 10;
  box-shadow: 0 6px 24px rgba(10, 36, 99, 0.25) !important;
  transition: none !important;
}

/* Phase 1: appear as small colored dots, like the floating background */
.face-slot {
  position: absolute;
  width: 12px;
  height: 12px;
  margin: -6px 0 0 -6px;
  border-radius: 50%;
  overflow: visible;
  border: none;
  box-shadow: none;
  opacity: 0;
  transition:
    opacity 4s ease,
    width 8s cubic-bezier(0.16, 1, 0.3, 1),
    height 8s cubic-bezier(0.16, 1, 0.3, 1),
    margin 8s cubic-bezier(0.16, 1, 0.3, 1),
    border 6s ease,
    box-shadow 6s ease;
  will-change: transform;
}

/* Phase 1: fade in as subtle dots */
.orbit-track.active .face-slot {
  opacity: 0.5;
}

.orbit-track.active .face-slot:nth-child(1) { transition-delay: 0s; }
.orbit-track.active .face-slot:nth-child(2) { transition-delay: 0.6s; }
.orbit-track.active .face-slot:nth-child(3) { transition-delay: 1.2s; }
.orbit-track.active .face-slot:nth-child(4) { transition-delay: 1.8s; }
.orbit-track.active .face-slot:nth-child(5) { transition-delay: 2.4s; }
.orbit-track.active .face-slot:nth-child(6) { transition-delay: 3s; }

/* Phase 2: morph dots into face bubbles */
.orbit-track.revealed .face-slot {
  width: 38px;
  height: 38px;
  margin: -19px 0 0 -19px;
  border: 2.5px solid rgba(255, 255, 255, 0.9);
  box-shadow: 0 2px 12px rgba(10, 36, 99, 0.12);
  opacity: 1;
  transition-delay: 0s;
}

.orbit-track.revealed .face-slot:nth-child(1) { transition-delay: 0s; }
.orbit-track.revealed .face-slot:nth-child(2) { transition-delay: 0.3s; }
.orbit-track.revealed .face-slot:nth-child(3) { transition-delay: 0.6s; }
.orbit-track.revealed .face-slot:nth-child(4) { transition-delay: 0.9s; }
.orbit-track.revealed .face-slot:nth-child(5) { transition-delay: 1.2s; }
.orbit-track.revealed .face-slot:nth-child(6) { transition-delay: 1.5s; }

/* Face image: hidden as dot, revealed gradually */
.face-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  border-radius: 50%;
  opacity: 0;
  transition: opacity 6s ease;
}

.orbit-track.revealed .face-img {
  opacity: 1;
}

.orbit-track.revealed .face-slot:nth-child(1) .face-img { transition-delay: 0.5s; }
.orbit-track.revealed .face-slot:nth-child(2) .face-img { transition-delay: 0.8s; }
.orbit-track.revealed .face-slot:nth-child(3) .face-img { transition-delay: 1.1s; }
.orbit-track.revealed .face-slot:nth-child(4) .face-img { transition-delay: 1.4s; }
.orbit-track.revealed .face-slot:nth-child(5) .face-img { transition-delay: 1.7s; }
.orbit-track.revealed .face-slot:nth-child(6) .face-img { transition-delay: 2s; }

/* === Speech Bubbles === */
.hero-speech-bubble {
  position: absolute;
  background: var(--navy);
  color: var(--white);
  font-size: 0.72rem;
  font-weight: 500;
  line-height: 1.4;
  padding: 0.5rem 0.7rem;
  border-radius: 10px;
  width: max-content;
  max-width: 190px;
  text-align: center;
  pointer-events: none;
  box-shadow: 0 4px 16px rgba(10, 36, 99, 0.25);
  z-index: 100;
}

.hero-speech-bubble::after {
  content: '';
  position: absolute;
  border: 5px solid transparent;
}

.noon-bubble {
  bottom: calc(100% + 6px);
  left: 50%;
  transform: translateX(-50%);
  white-space: normal;
}

.noon-bubble::after {
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border-top-color: var(--navy);
}

.face-bubble {
  bottom: calc(100% + 6px);
  left: 50%;
  transform: translateX(-50%);
}

.face-bubble::after {
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border-top-color: var(--navy);
}

.bubble-enter-active {
  transition: all 0.25s cubic-bezier(0.16, 1, 0.3, 1);
}

.bubble-leave-active {
  transition: all 0.15s ease-in;
}

.bubble-enter-from {
  opacity: 0;
  transform: translateX(-50%) translateY(6px) scale(0.92);
}

.bubble-leave-to {
  opacity: 0;
  transform: translateX(-50%) translateY(4px) scale(0.95);
}

/* === End Orbit System === */

.hero-author .by {
  display: block;
  font-size: 0.9rem;
  color: var(--gray-400);
  margin-bottom: 0.25rem;
  position: relative;
  z-index: 4;
}

.hero-author .name {
  font-family: 'Playfair Display', serif;
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--blue);
  position: relative;
  z-index: 4;
}

.hero-meta {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1.5rem;
  margin-bottom: 2.5rem;
}

.meta-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: var(--gray-600);
  font-size: 0.95rem;
  font-weight: 500;
}

.meta-item svg {
  color: var(--blue-mid);
  flex-shrink: 0;
}

.hero-buttons {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.75rem;
}

.hero-cta {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.85rem 1.75rem;
  border-radius: 50px;
  font-weight: 600;
  font-size: 0.9rem;
  letter-spacing: 0.02em;
  transition: all 0.3s ease;
  cursor: pointer;
  border: none;
  font-family: inherit;
  text-decoration: none;
}

.hero-cta.rsvp {
  background: var(--lime);
  color: var(--navy);
  box-shadow: 0 4px 24px rgba(197, 232, 76, 0.4);
  padding: 1rem 2.5rem;
  font-size: 1.1rem;
  font-weight: 700;
  letter-spacing: 0.04em;
  animation: rsvpGlow 2s ease-in-out infinite;
}

@keyframes rsvpGlow {
  0%, 100% { box-shadow: 0 4px 24px rgba(197, 232, 76, 0.4); }
  50% { box-shadow: 0 6px 36px rgba(197, 232, 76, 0.6), 0 0 60px rgba(197, 232, 76, 0.15); }
}

.hero-cta.rsvp:hover {
  background: var(--lime-light);
  transform: translateY(-3px) scale(1.03);
  box-shadow: 0 8px 40px rgba(197, 232, 76, 0.5);
  color: var(--navy);
}

.hero-cta.calendar {
  background: var(--navy);
  color: var(--white);
  box-shadow: 0 4px 20px rgba(10, 36, 99, 0.2);
}

.hero-cta.calendar:hover {
  background: var(--blue);
  color: var(--white);
  transform: translateY(-2px);
  box-shadow: 0 6px 30px rgba(10, 36, 99, 0.3);
}

.hero-cta.secondary {
  background: transparent;
  color: var(--blue);
  border: 2px solid var(--blue-pale);
  padding: 0.75rem 1.5rem;
}

.hero-cta.secondary:hover {
  border-color: var(--blue);
  color: var(--navy);
  transform: translateY(-2px);
}

.hero-cta.secondary svg {
  transition: transform 0.3s ease;
}

.hero-cta.secondary:hover svg {
  transform: translateY(3px);
}

@media (max-width: 600px) {
  .hero-meta {
    flex-direction: column;
    align-items: center;
    gap: 0.75rem;
  }

  .hero-buttons {
    flex-direction: column;
    align-items: center;
  }

  .hero-cta {
    width: 100%;
    max-width: 250px;
    justify-content: center;
  }

  .orbit-track.revealed .face-slot {
    width: 30px;
    height: 30px;
    margin: -15px 0 0 -15px;
  }
}
</style>
