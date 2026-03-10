<script setup>
import { ref, onMounted } from 'vue'

const visible = ref(false)

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) visible.value = true
    },
    { threshold: 0.1 }
  )
  const el = document.getElementById('schedule')
  if (el) observer.observe(el)
})

const phases = [
  {
    key: 'defense',
    label: 'Defense Ceremony',
    note: 'Please arrive by 09:45: no entry after 10:00',
    events: [
      { time: '10:00', label: 'Defense Begins', big: true },
      { time: '10:10', label: 'End of Presentation' },
      { time: '10:15', label: 'Committee Q&A' },
      { time: '11:00', label: 'End of Defense', big: true },
    ],
  },
  {
    key: 'social',
    label: 'Celebration',
    events: [
      { time: '11:10', label: 'Borrel Downstairs (same building)' },
      { time: '12:00', label: 'Borrel End' },
    ],
  },
  {
    key: 'party',
    label: 'Evening',
    events: [
      { time: '18:30', label: 'Party — Amstelhaven', big: true },
    ],
  },
]
</script>

<template>
  <section id="schedule" :class="['schedule', { visible }]">
    <div class="schedule-inner">
      <div class="section-badge">The Day</div>
      <h2>Thursday, 9 April 2026</h2>

      <div class="journey">
        <div
          v-for="(phase, pi) in phases"
          :key="phase.key"
          :class="['phase', phase.key]"
          :style="{ animationDelay: (pi * 0.12) + 's' }"
        >
          <div class="phase-label">
            <span class="phase-label-text">{{ phase.label }}</span>
            <span class="phase-line" />
          </div>

          <p v-if="phase.note" class="phase-note">{{ phase.note }}</p>

          <div class="phase-events">
            <div
              v-for="(event, ei) in phase.events"
              :key="ei"
              :class="['event', { big: event.big }]"
            >
              <span class="event-time">{{ event.time }}</span>
              <span class="event-dot" />
              <span class="event-label">{{ event.label }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.schedule {
  padding: 7rem 2rem 6rem;
  background: var(--white);
  position: relative;
}

.schedule::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--gray-200), transparent);
}

.schedule-inner {
  max-width: 720px;
  margin: 0 auto;
  text-align: center;
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.schedule.visible .schedule-inner {
  opacity: 1;
  transform: translateY(0);
}

.section-badge {
  display: inline-block;
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--blue);
  background: rgba(21, 101, 192, 0.06);
  padding: 0.4rem 1.2rem;
  border-radius: 20px;
  margin-bottom: 1.25rem;
}

h2 {
  font-size: clamp(2rem, 4vw, 2.8rem);
  color: var(--navy);
  margin-bottom: 4rem;
}

/* === Journey === */
.journey {
  text-align: left;
  display: flex;
  flex-direction: column;
  gap: 3rem;
}

.schedule.visible .phase {
  animation: fadeUp 0.6s ease forwards;
  opacity: 0;
}

@keyframes fadeUp {
  from { opacity: 0; transform: translateY(16px); }
  to { opacity: 1; transform: translateY(0); }
}

/* === Phase === */
.phase-label {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1.25rem;
}

.phase-label-text {
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--gray-400);
  white-space: nowrap;
  flex-shrink: 0;
}

.phase.defense .phase-label-text { color: var(--blue); }
.phase.party .phase-label-text { color: var(--navy); }

.phase-line {
  flex: 1;
  height: 1px;
  background: var(--gray-200);
}

.phase-note {
  font-size: 0.82rem;
  color: var(--blue-mid);
  font-style: italic;
  margin: -0.75rem 0 1rem 0.5rem;
  padding-left: 0.75rem;
  border-left: 2px solid var(--blue-pale);
}

.phase.defense .phase-line {
  background: linear-gradient(90deg, var(--blue-pale), transparent);
}

.phase.party .phase-line {
  background: linear-gradient(90deg, var(--lime), transparent);
}

/* === Events === */
.phase-events {
  display: flex;
  flex-direction: column;
  gap: 0;
  padding-left: 0.5rem;
}

.event {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 0.7rem 0.75rem;
  border-radius: 10px;
  transition: all 0.25s ease;
}

.event:hover {
  background: rgba(21, 101, 192, 0.03);
}

.event-time {
  width: 52px;
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--gray-400);
  font-variant-numeric: tabular-nums;
  flex-shrink: 0;
  text-align: right;
}

.event.big .event-time {
  color: var(--blue);
  font-weight: 700;
}

.phase.party .event-time {
  color: var(--navy);
}

.event-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--gray-200);
  flex-shrink: 0;
  transition: all 0.3s ease;
}

.event:hover .event-dot {
  transform: scale(1.5);
}

.event.big .event-dot {
  width: 12px;
  height: 12px;
  background: var(--blue);
  box-shadow: 0 0 0 4px rgba(21, 101, 192, 0.1);
}

.phase.defense .event.big .event-dot {
  background: var(--blue);
  box-shadow: 0 0 0 4px rgba(21, 101, 192, 0.12);
}

.phase.party .event-dot {
  background: var(--lime);
}

.phase.party .event.big .event-dot {
  width: 12px;
  height: 12px;
  background: var(--lime);
  box-shadow: 0 0 0 4px rgba(197, 232, 76, 0.2);
}

.event-label {
  font-size: 0.95rem;
  color: var(--gray-600);
  font-weight: 400;
}

.event.big .event-label {
  font-size: 1.05rem;
  font-weight: 600;
  color: var(--navy);
}

.phase.party .event.big .event-label {
  color: var(--navy);
  font-size: 1.1rem;
}

@media (max-width: 600px) {
  .journey {
    gap: 2.25rem;
  }

  .event {
    gap: 0.75rem;
    padding: 0.6rem 0.5rem;
  }

  .event-time {
    width: 44px;
    font-size: 0.8rem;
  }

  .event-label {
    font-size: 0.88rem;
  }

  .event.big .event-label {
    font-size: 0.95rem;
  }

  h2 {
    margin-bottom: 3rem;
  }
}
</style>
