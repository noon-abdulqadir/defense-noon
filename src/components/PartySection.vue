<script setup>
import { ref, onMounted } from 'vue'

const visible = ref(false)

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) visible.value = true
    },
    { threshold: 0.2 }
  )
  const el = document.getElementById('party')
  if (el) observer.observe(el)
})
</script>

<template>
  <section id="party" :class="['party', { visible }]">
    <div class="party-dots" aria-hidden="true">
      <span v-for="i in 20" :key="i" class="p-dot" :style="{
        left: (Math.random() * 100) + '%',
        top: (Math.random() * 100) + '%',
        width: (Math.random() * 30 + 10) + 'px',
        height: (Math.random() * 30 + 10) + 'px',
        background: ['#C5E84C', '#D4F06A', '#1565C0', '#7CB9E8', '#0A2463'][i % 5],
        opacity: Math.random() * 0.15 + 0.05,
        animationDelay: (Math.random() * 5) + 's',
      }" />
    </div>

    <div class="party-inner">
      <div class="party-badge">Let's Celebrate!</div>
      <h2>Evening Party</h2>
      <p class="party-subtitle">
        Join us to celebrate Noon becoming
        <span class="dr">Dr. Noon M.F. Abdulqadir</span>
      </p>

      <div class="party-card">
        <div class="party-info-row">
          <div class="party-info">
            <div class="party-icon">
              <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>
            </div>
            <div>
              <span class="info-label">Date</span>
              <span class="info-value">Thursday, 9 April 2026</span>
            </div>
          </div>

          <div class="party-divider" />

          <div class="party-info">
            <div class="party-icon">
              <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
            </div>
            <div>
              <span class="info-label">Time</span>
              <span class="info-value">6:30 PM</span>
            </div>
          </div>

          <div class="party-divider" />

          <div class="party-info">
            <div class="party-icon">
              <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>
            </div>
            <div>
              <span class="info-label">Location</span>
              <span class="info-value">Cafe de Jaren</span>
            </div>
          </div>
        </div>

        <div class="party-address">
          <p class="tba-text">Details to be announced</p>
        </div>

      </div>

    </div>
  </section>
</template>

<style scoped>
.party {
  padding: 7rem 2rem;
  background: var(--navy);
  position: relative;
  overflow: hidden;
}

.party-dots {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.p-dot {
  position: absolute;
  border-radius: 50%;
  animation: floatParty 20s linear infinite;
}

@keyframes floatParty {
  0%, 100% { transform: translate(0, 0); }
  33% { transform: translate(15px, -10px); }
  66% { transform: translate(-10px, 15px); }
}

.party-inner {
  max-width: 700px;
  margin: 0 auto;
  text-align: center;
  position: relative;
  z-index: 1;
  opacity: 0;
  transform: translateY(40px);
  transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.party.visible .party-inner {
  opacity: 1;
  transform: translateY(0);
}

.party-badge {
  display: inline-block;
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--navy);
  background: var(--lime);
  padding: 0.4rem 1rem;
  border-radius: 20px;
  margin-bottom: 1.5rem;
}

h2 {
  font-size: clamp(2rem, 4vw, 3rem);
  color: var(--white);
  margin-bottom: 1rem;
}

.party-subtitle {
  color: var(--blue-pale);
  font-size: 1.15rem;
  margin-bottom: 2.5rem;
  line-height: 1.6;
}

.dr {
  display: block;
  font-family: 'Playfair Display', serif;
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--lime);
  margin-top: 0.5rem;
  font-style: italic;
}

.party-card {
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 20px;
  padding: 2.5rem;
}

.party-info-row {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 2.5rem;
  margin-bottom: 1.5rem;
}

.party-info {
  display: flex;
  align-items: center;
  gap: 1rem;
  text-align: left;
}

.party-icon {
  color: var(--lime);
  flex-shrink: 0;
}

.info-label {
  display: block;
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--blue-light);
  font-weight: 500;
}

.info-value {
  display: block;
  font-size: 1.25rem;
  font-weight: 700;
  color: var(--white);
}

.party-divider {
  width: 1px;
  height: 50px;
  background: rgba(255, 255, 255, 0.15);
}

.party-address {
  padding-top: 1.5rem;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.party-address p {
  color: var(--blue-pale);
  font-size: 0.95rem;
  margin-bottom: 0.75rem;
}

.tba-text {
  color: var(--lime) !important;
  font-size: 1.1rem !important;
  font-weight: 600;
  font-style: italic;
  margin-bottom: 0 !important;
  padding: 0.5rem 0;
}

.party-map-container {
  width: 100%;
  height: 180px;
  border-radius: 12px;
  overflow: hidden;
  margin-bottom: 0.75rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.party-map-link {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  color: var(--lime);
  font-weight: 600;
  font-size: 0.9rem;
  transition: all 0.2s ease;
}

.party-map-link:hover {
  color: var(--lime-light);
}

@media (max-width: 550px) {
  .party-info-row {
    flex-direction: column;
    gap: 1.5rem;
  }

  .party-divider {
    width: 60px;
    height: 1px;
  }

  .party-card {
    padding: 1.5rem;
  }
}
</style>
