<script setup>
import { ref, onMounted, computed } from 'vue'

const mapsApiKey = import.meta.env.VITE_MAPS_API_KEY
const mapSrc = computed(() =>
  mapsApiKey
    ? `https://www.google.com/maps/embed/v1/place?key=${mapsApiKey}&q=Agnietenkapel,Oudezijds+Voorburgwal+229,Amsterdam&zoom=16`
    : null
)

const visible = ref(false)

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) visible.value = true
    },
    { threshold: 0.2 }
  )
  const el = document.getElementById('venue')
  if (el) observer.observe(el)
})
</script>

<template>
  <section id="venue" :class="['venue', { visible }]">
    <div class="venue-inner">
      <div class="section-badge">Venue</div>
      <h2>Agnietenkapel</h2>
      <p class="venue-subtitle">A historic 15th-century chapel in the heart of Amsterdam</p>

      <div class="venue-grid">
        <div class="venue-info">
          <div class="info-card">
            <div class="info-icon">
              <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>
            </div>
            <div>
              <h3>Address</h3>
              <p>Oudezijds Voorburgwal 229<br>1012 EZ Amsterdam</p>
            </div>
          </div>

          <div class="info-card">
            <div class="info-icon">
              <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
            </div>
            <div>
              <h3>Defense Starts</h3>
              <p>10:00 AM sharp<br>Please arrive by 09:45</p>
            </div>
          </div>

          <div class="info-card">
            <div class="info-icon warn">
              <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M10.29 3.86L1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/><line x1="12" y1="9" x2="12" y2="13"/><line x1="12" y1="17" x2="12.01" y2="17"/></svg>
            </div>
            <div>
              <h3>Important</h3>
              <p>No entry after 10:00 AM<br>Doors close when defense begins</p>
            </div>
          </div>
        </div>

        <div class="venue-map">
          <div class="map-container">
            <iframe
              v-if="mapSrc"
              :src="mapSrc"
              width="100%"
              height="100%"
              style="border:0"
              allowfullscreen=""
              loading="lazy"
              referrerpolicy="no-referrer-when-downgrade"
            />
            <div v-else class="map-fallback">
              <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>
              <span>Oudezijds Voorburgwal 229, Amsterdam</span>
            </div>
          </div>
          <a
            href="https://maps.google.com/?q=Agnietenkapel+Oudezijds+Voorburgwal+229+Amsterdam"
            target="_blank"
            rel="noopener"
            class="map-link"
          >
            Open in Google Maps
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/></svg>
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.venue {
  padding: 7rem 2rem;
  background: var(--white);
}

.venue-inner {
  max-width: 1000px;
  margin: 0 auto;
  text-align: center;
  opacity: 0;
  transform: translateY(40px);
  transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.venue.visible .venue-inner {
  opacity: 1;
  transform: translateY(0);
}

.section-badge {
  display: inline-block;
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--blue);
  background: rgba(21, 101, 192, 0.08);
  padding: 0.4rem 1rem;
  border-radius: 20px;
  margin-bottom: 1.5rem;
}

h2 {
  font-size: clamp(2rem, 4vw, 3rem);
  color: var(--navy);
  margin-bottom: 0.5rem;
}

.venue-subtitle {
  color: var(--gray-400);
  margin-bottom: 3rem;
  font-size: 1.05rem;
  font-style: italic;
}

.venue-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2.5rem;
  text-align: left;
}

.venue-info {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.info-card {
  display: flex;
  gap: 1rem;
  padding: 1.25rem;
  background: var(--off-white);
  border-radius: 12px;
  border: 1px solid var(--gray-200);
  transition: all 0.3s ease;
}

.info-card:hover {
  border-color: var(--blue-light);
  box-shadow: 0 4px 20px rgba(10, 36, 99, 0.06);
}

.info-icon {
  flex-shrink: 0;
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(21, 101, 192, 0.08);
  border-radius: 10px;
  color: var(--blue);
}

.info-icon.warn {
  background: rgba(197, 232, 76, 0.2);
  color: #7B5E00;
}

.info-card h3 {
  font-family: 'Inter', sans-serif;
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--navy);
  margin-bottom: 0.25rem;
}

.info-card p {
  font-size: 0.9rem;
  color: var(--gray-600);
  line-height: 1.5;
}

.venue-map {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.map-container {
  width: 100%;
  height: 280px;
  border-radius: 16px;
  overflow: hidden;
  border: 1px solid var(--gray-200);
  background: var(--gray-100);
}

.map-link {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.9rem;
  font-weight: 500;
  color: var(--blue);
}

.map-link:hover {
  color: var(--navy);
}

.map-fallback {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  color: var(--gray-400);
  font-size: 0.9rem;
}

@media (max-width: 768px) {
  .venue-grid {
    grid-template-columns: 1fr;
  }
}
</style>
