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
    quotes: [
      'That ad was microtargeted at you specifically. Yes, you.',
      'The algorithm knows your politics better than your therapist.',
      'I track political ads so you don\'t have to. You\'re welcome.',
      'Your feed isn\'t random. It never was.',
      'Every campaign ad has a strategy. I just read the receipts.',
      'I scraped two million ads and all I got was this PhD.',
      'Dark money, dark ads, bright spreadsheets.',
      'Facebook knows who you\'ll vote for before you do.',
      'Ad transparency reports are my beach reads.',
      'That "grassroots" campaign? I traced it to a consultancy.',
      'Your political identity is worth $0.03 per impression.',
      'I turned Meta\'s ad library into a crime scene investigation.',
      'Sponsored content is just propaganda with better UX.',
      'Every election has a paper trail. It\'s in the ad spend.',
      'The best political fiction? Targeted ad disclaimers.',
      'Platforms don\'t run campaigns. They just charge for them.',
      'Your data profile is a campaign donor and doesn\'t know it.',
      'I found partisan ads disguised as weather updates. Seriously.',
      'Political advertising: where creativity meets manipulation.',
      'If you haven\'t been microtargeted, you\'re not a swing voter.',
    ],
  },
  {
    name: 'Delaney Peterson',
    initial: 'DP',
    color: '#3E8EDE',
    photo: 'https://hotpolitics.eu/wp-content/uploads/2025/12/Delaney_Peterson_2025-scaled-center-top-500x500.jpg',
    quotes: [
      'Feeling lonely? A populist party might be sliding into your DMs.',
      'Your doom-scrolling habits say a lot about your vote.',
      'Hot take: emotions explain elections better than economics.',
      'I study why angry people vote angry. It\'s not rocket science.',
      'The psychology of populism? It\'s complicated. Like your ex.',
      'Emotional contagion is just vibes with a sample size.',
      'Populists promise belonging. No refunds.',
      'Your rage-click is someone\'s electoral strategy.',
      'Loneliness is a political condition. I have the regression.',
      'Affective polarization: when you hate the other side more than you like yours.',
      'I measure feelings for a living. Yes, with statistics.',
      'Social isolation + social media = political radicalization. Math checks out.',
      'Your anxiety is statistically significant at the .001 level.',
      'Grief, anger, fear — the real swing voters.',
      'Populist rhetoric works because it feels true. That\'s the problem.',
      'The attention economy runs on outrage. I measure the fuel.',
      'Your cortisol levels have electoral consequences.',
      'I study hot politics. It\'s literally in the URL.',
      'The personal is political. The emotional is electoral.',
      'Doom-scrolling: the world\'s most depressing civic engagement.',
    ],
  },
  {
    name: 'Roeland Dubèl',
    initial: 'RD',
    color: '#0A2463',
    photo: 'https://www.uva.nl/binaries/_ht_1747924741618/content/documents/personalpages/d/u/r.dubel/r.dubel',
    quotes: [
      'Do you trust this quote? That\'s basically my whole PhD.',
      'Plot twist: transparency doesn\'t always increase trust.',
      'I study media trust so you can distrust me more effectively.',
      'Journalists try so hard. Readers still say "fake news."',
      '"Source: trust me bro" — the opposite of my research.',
      'Trust takes years to build and one headline to destroy.',
      'More information ≠ more trust. I have a paper on it.',
      '"I saw it on the internet" is not a citation. Except in my data.',
      'Credibility is a spectrum. Most people are at the wrong end.',
      'I study why you believe strangers on Reddit but not Reuters.',
      'News literacy: the gym membership everyone buys but never uses.',
      'My PhD is basically: "Why won\'t people trust good journalism?"',
      'Trust erosion is slow. Distrust goes viral.',
      'The most trusted news source? Whatever confirms your priors.',
      '"But I read it somewhere" — the rallying cry of misinformation.',
      'I quantify skepticism. It\'s an abundant resource.',
      'Institutional trust: the graph only goes down.',
      'Fact-checkers exist. Being believed is another story.',
      'Your news avoidance is my research pipeline.',
      'If trust were simple, communication scientists would be out of a job.',
    ],
  },
  {
    name: 'Chiara Vargiu',
    initial: 'CV',
    color: '#7CB9E8',
    photo: 'https://cdn.bsky.app/img/avatar/plain/did:plc:squv6a7sg6vheny4xnanktgv/bafkreichklk5pv7lb6yll3r7vvtbltld6en52ur3v6huyyct3btyvxnrf4@jpeg',
    quotes: [
      'Politicians being rude? That\'s not a bug, it\'s my dataset.',
      'I measure political trash-talk for a living. Dream job tbh.',
      'Incivility in politics is rising. My sample size is thriving.',
      'Your "hot take" on Twitter? I have a codebook for that.',
      'Democracy dies in darkness. Also in comment sections.',
      'I annotate insults so you don\'t have to.',
      'Parliamentary debates are just academic Twitter with more rules.',
      'Name-calling in parliament is up 300%. I counted.',
      'Your reply-guy energy? Peer-reviewed.',
      'Political rudeness: the only bipartisan consensus.',
      'Trolling is a rhetorical strategy. An effective one.',
      'I turned hate-scrolling into a methodology.',
      'Discourse norms? In this political climate?',
      'My intercoder reliability on detecting shade is impeccable.',
      'Politicians used to disagree politely. I have the exact year it stopped.',
      '"With all due respect" — the last polite phrase in politics.',
      'Toxicity is a feature, not a bug, of online politics.',
      'I read comment sections for science. My therapist is concerned.',
      'If incivility were currency, politics would have no debt.',
      'The Overton window of rudeness has been shattered. I measured the shards.',
    ],
  },
  {
    name: 'Ernesto de León Williams',
    initial: 'EW',
    color: '#C5E84C',
    photo: 'https://www.ernesto-deleon.com/authors/admin/avatar_hu614f9917ce8b72d4443466d9e6bcba9e_221201_270x270_fill_q75_lanczos_center.jpg',
    quotes: [
      'Your filter bubble is showing. Don\'t worry, I can see it.',
      'I trace how news travels online. It takes weird detours.',
      'The algorithm curates your reality. I curate the algorithm.',
      'Shared political reality? In this economy?',
      'I study information flows. Yours is mostly memes.',
      'Echo chambers have great acoustics but terrible diversity.',
      'Your news bubble is smaller than you think. And cozier.',
      'I mapped the information landscape. It looks like a maze.',
      'Selective exposure: you only see what you already believe.',
      'Fragmented media = fragmented society. I did the math.',
      'The algorithm doesn\'t show you the whole picture. I do.',
      'Cross-cutting news exposure is my unrequited love.',
      'Information silos are real. I built the floor plan.',
      'Your "well-informed opinion" has a sample size of one timeline.',
      'Media diversity is a public good. Also extremely rare.',
      'I study what you never see in your feed. It\'s a lot.',
      'Misinformation travels at the speed of outrage.',
      'Two people can use the same app and live in different realities.',
      'Your news diet has the nutritional value of a vending machine.',
      'I follow the information so you know what you\'re missing. (A lot.)',
    ],
  },
]

const hoveredIndex = ref(-1)
const currentQuoteIndex = ref({})

function onHover(i) {
  // Pick a random quote, different from last time
  const quotes = paranymphs[i].quotes
  let idx = Math.floor(Math.random() * quotes.length)
  if (currentQuoteIndex.value[i] === idx) {
    idx = (idx + 1) % quotes.length
  }
  currentQuoteIndex.value[i] = idx
  hoveredIndex.value = i
}

function onLeave() {
  hoveredIndex.value = -1
}

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
          @mouseenter="onHover(i)"
          @mouseleave="onLeave"
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
          <transition name="bubble">
            <div v-if="hoveredIndex === i" class="speech-bubble">
              {{ p.quotes[currentQuoteIndex[i] ?? 0] }}
            </div>
          </transition>
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

/* Speech Bubble */
.para-card {
  position: relative;
}

.speech-bubble {
  position: absolute;
  bottom: calc(100% + 12px);
  left: 50%;
  transform: translateX(-50%);
  background: var(--navy);
  color: var(--white);
  font-size: 0.78rem;
  font-weight: 500;
  line-height: 1.45;
  padding: 0.65rem 0.9rem;
  border-radius: 12px;
  width: max-content;
  max-width: 220px;
  text-align: center;
  pointer-events: none;
  box-shadow: 0 4px 20px rgba(10, 36, 99, 0.2);
  z-index: 10;
}

.speech-bubble::after {
  content: '';
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border: 6px solid transparent;
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
