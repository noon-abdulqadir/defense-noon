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
  const el = document.getElementById('paranymphs')
  if (el) observer.observe(el)
})

const paranymphs = [
  {
    name: 'Fabio Votta',
    initial: 'FV',
    color: '#1565C0',
    photo: 'https://algosoc.org/uploads/_card/fabio-votta-1.jpg',
  },
  {
    name: 'Delaney Peterson',
    initial: 'DP',
    color: '#3E8EDE',
    photo: 'https://hotpolitics.eu/wp-content/uploads/2025/12/Delaney_Peterson_2025-scaled-center-top-500x500.jpg',
  },
  {
    name: 'Roeland Dubèl',
    initial: 'RD',
    color: '#0A2463',
    photo: 'https://www.uva.nl/binaries/_ht_1747924741618/content/documents/personalpages/d/u/r.dubel/r.dubel',
  },
  {
    name: 'Chiara Vargiu',
    initial: 'CV',
    color: '#7CB9E8',
    photo: 'https://cdn.bsky.app/img/avatar/plain/did:plc:squv6a7sg6vheny4xnanktgv/bafkreichklk5pv7lb6yll3r7vvtbltld6en52ur3v6huyyct3btyvxnrf4@jpeg',
  },
  {
    name: 'Ernesto de León Williams',
    initial: 'EW',
    color: '#C5E84C',
    photo: 'https://www.ernesto-deleon.com/authors/admin/avatar_hu614f9917ce8b72d4443466d9e6bcba9e_221201_270x270_fill_q75_lanczos_center.jpg',
  },
]

const imgErrors = ref({})
function onImgError(i) {
  imgErrors.value[i] = true
}
</script>

<template>
  <section id="paranymphs" :class="['paranymphs', { visible }]">
    <div class="paranymphs-inner">
      <div class="section-badge">Paranymphs</div>
      <h2>The Support Team</h2>
      <p class="para-subtitle">Standing by Noon's side on this special day</p>

      <div class="para-grid">
        <div
          v-for="(p, i) in paranymphs"
          :key="i"
          class="para-card"
          :style="{ animationDelay: (i * 0.1) + 's' }"
        >
          <div class="para-avatar-wrap">
            <img
              v-if="p.photo && !imgErrors[i]"
              :src="p.photo"
              :alt="p.name"
              class="para-photo"
              @error="onImgError(i)"
              loading="lazy"
            />
            <div v-else class="para-avatar" :style="{ background: p.color }">
              <span :style="{ color: p.color === '#C5E84C' ? '#0A2463' : '#FFFFFF' }">{{ p.initial }}</span>
            </div>
          </div>
          <span class="para-name">{{ p.name }}</span>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.paranymphs {
  padding: 7rem 2rem;
  background: var(--off-white);
  position: relative;
  overflow: hidden;
}

.paranymphs::before {
  content: '';
  position: absolute;
  width: 300px;
  height: 300px;
  border-radius: 50%;
  background: rgba(197, 232, 76, 0.08);
  top: -100px;
  right: -100px;
}

.paranymphs::after {
  content: '';
  position: absolute;
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background: rgba(21, 101, 192, 0.05);
  bottom: -60px;
  left: -60px;
}

.paranymphs-inner {
  max-width: 900px;
  margin: 0 auto;
  text-align: center;
  position: relative;
  z-index: 1;
  opacity: 0;
  transform: translateY(40px);
  transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.paranymphs.visible .paranymphs-inner {
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

.para-subtitle {
  color: var(--gray-400);
  margin-bottom: 3rem;
  font-size: 1.05rem;
}

.para-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 2.5rem;
}

.para-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.75rem;
  transition: all 0.3s ease;
  width: 130px;
}

.paranymphs.visible .para-card {
  animation: popIn 0.5s ease forwards;
  opacity: 0;
}

@keyframes popIn {
  from { opacity: 0; transform: scale(0.8); }
  to { opacity: 1; transform: scale(1); }
}

.para-card:hover {
  transform: translateY(-4px);
}

.para-avatar-wrap {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  border: 3px solid var(--white);
}

.para-card:hover .para-avatar-wrap {
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.15);
  transform: scale(1.05);
}

.para-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.para-avatar {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Playfair Display', serif;
  font-size: 1.5rem;
  font-weight: 600;
}

.para-name {
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--navy);
  line-height: 1.3;
}

@media (max-width: 600px) {
  .para-grid {
    gap: 1.5rem;
  }

  .para-card {
    width: 110px;
  }

  .para-avatar-wrap {
    width: 80px;
    height: 80px;
  }

  .para-avatar {
    font-size: 1.2rem;
  }
}
</style>
