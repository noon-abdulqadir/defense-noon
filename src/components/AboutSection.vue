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
  const el = document.getElementById('about')
  if (el) observer.observe(el)
})
</script>

<template>
  <section id="about" :class="['about', { visible }]">
    <div class="about-inner">
      <div class="section-badge">About the Dissertation</div>
      <h2>Hiring in the Digital Society</h2>
      <p class="about-subtitle">
        Content and Consequences of Gender and Age Stereotypes in Job Advertisements
      </p>

      <div class="chapters">
        <div class="chapter-card">
          <div class="chapter-num">Ch. 2</div>
          <h3>Sourcing Phase: Stereotypical Frames</h3>
          <p>
            Stereotypical frames in online job ads mirror stereotypes of social groups dominant in said occupational domain.
          </p>
        </div>
        <div class="chapter-card">
          <div class="chapter-num">Ch. 3</div>
          <h3>Sourcing Phase: Platform Delivery</h3>
          <p>
            Online platforms deliver job ads along stereotypical lines, but country-level individualistic and egalitarian norms alter delivery patterns.
          </p>
        </div>
        <div class="chapter-card">
          <div class="chapter-num">Ch. 4</div>
          <h3>Selection Phase: AI Recommendations</h3>
          <p>
            Candidate selection is not fully dictated by AI recommendations, but these do influence more subtle candidate fit assessments.
          </p>
        </div>
      </div>

      <p class="about-conclusion">
        The findings highlight the contextuality of social group stereotypes in the occupational domain and the breadth of considerations for understanding, defining, and mitigating algorithmic bias in hiring.
      </p>
    </div>
  </section>
</template>

<style scoped>
.about {
  padding: 7rem 2rem;
  background: var(--white);
  position: relative;
}

.about::before {
  content: '';
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 3px;
  background: linear-gradient(90deg, var(--blue), var(--lime));
  border-radius: 2px;
}

.about-inner {
  max-width: 1000px;
  margin: 0 auto;
  text-align: center;
  opacity: 0;
  transform: translateY(40px);
  transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.about.visible .about-inner {
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
  margin-bottom: 0.75rem;
}

.about-subtitle {
  font-family: 'Playfair Display', serif;
  font-style: italic;
  color: var(--blue-mid);
  font-size: 1.15rem;
  margin-bottom: 3rem;
}

.chapters {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
  margin-bottom: 3rem;
  text-align: left;
}

.chapter-card {
  background: var(--off-white);
  border: 1px solid var(--gray-200);
  border-radius: 16px;
  padding: 2rem;
  transition: all 0.3s ease;
}

.chapter-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 40px rgba(10, 36, 99, 0.08);
  border-color: var(--blue-light);
}

.chapter-num {
  display: inline-block;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--white);
  background: var(--blue);
  padding: 0.25rem 0.75rem;
  border-radius: 20px;
  margin-bottom: 1rem;
}

.chapter-card:nth-child(2) .chapter-num {
  background: var(--blue-mid);
}

.chapter-card:nth-child(3) .chapter-num {
  background: var(--navy);
}

.chapter-card h3 {
  font-family: 'Inter', sans-serif;
  font-size: 1.05rem;
  font-weight: 600;
  color: var(--navy);
  margin-bottom: 0.75rem;
}

.chapter-card p {
  color: var(--gray-600);
  font-size: 0.95rem;
  line-height: 1.6;
}

.about-conclusion {
  max-width: 700px;
  margin: 0 auto;
  color: var(--gray-600);
  font-size: 1rem;
  line-height: 1.7;
  padding: 1.5rem 2rem;
  border-left: 3px solid var(--lime);
  text-align: left;
  background: linear-gradient(90deg, rgba(197, 232, 76, 0.06), transparent);
  border-radius: 0 8px 8px 0;
}
</style>
