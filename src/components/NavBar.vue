<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const scrolled = ref(false)
const menuOpen = ref(false)

const handleScroll = () => {
  scrolled.value = window.scrollY > 50
}

const toggleMenu = () => {
  menuOpen.value = !menuOpen.value
}

const closeMenu = () => {
  menuOpen.value = false
}

onMounted(() => window.addEventListener('scroll', handleScroll))
onUnmounted(() => window.removeEventListener('scroll', handleScroll))

const navLinks = [
  { href: '#schedule', label: 'Schedule' },
  { href: '#venue', label: 'Venue' },
  { href: '#paranymphs', label: 'Paranymphs' },
  { href: '#party', label: 'Party' },
  { href: '#about', label: 'Dissertation' },
]
</script>

<template>
  <nav :class="['navbar', { scrolled }]">
    <div class="nav-inner">
      <a href="#" class="nav-logo" @click.prevent="closeMenu">
        <span class="logo-dot d1"></span>
        <span class="logo-dot d2"></span>
        <span class="logo-dot d3"></span>
      </a>
      <button class="menu-toggle" @click="toggleMenu" :aria-expanded="menuOpen">
        <span :class="['hamburger', { open: menuOpen }]">
          <span></span><span></span><span></span>
        </span>
      </button>
      <ul :class="['nav-links', { open: menuOpen }]">
        <li v-for="link in navLinks" :key="link.href">
          <a :href="link.href" @click="closeMenu">{{ link.label }}</a>
        </li>
        <li>
          <a href="https://www.noon-abdulqadir.com" target="_blank" rel="noopener" class="nav-academic">
            Noon's Academic Website
            <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" aria-hidden="true"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/></svg>
          </a>
        </li>
      </ul>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  padding: 1rem 2rem;
  transition: all 0.3s ease;
}

.navbar.scrolled {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(12px);
  box-shadow: 0 1px 20px rgba(10, 36, 99, 0.08);
  padding: 0.6rem 2rem;
}

.nav-inner {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.nav-logo {
  display: flex;
  gap: 6px;
  align-items: center;
  padding: 6px;
}

.logo-dot {
  border-radius: 50%;
  transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.logo-dot.d1 {
  background: var(--blue);
  width: 14px;
  height: 14px;
  animation: orbitD1 4s ease-in-out infinite;
}

.logo-dot.d2 {
  background: var(--lime);
  width: 10px;
  height: 10px;
  animation: orbitD2 4s ease-in-out infinite;
}

.logo-dot.d3 {
  background: var(--navy);
  width: 8px;
  height: 8px;
  animation: orbitD3 4s ease-in-out infinite;
}

@keyframes orbitD1 {
  0%, 100% { transform: translate(0, 0) scale(1); }
  25% { transform: translate(2px, -4px) scale(1.1); }
  50% { transform: translate(0, 0) scale(1); }
  75% { transform: translate(-2px, 3px) scale(0.95); }
}

@keyframes orbitD2 {
  0%, 100% { transform: translate(0, 0) scale(1); }
  25% { transform: translate(-3px, 3px) scale(0.9); }
  50% { transform: translate(4px, -2px) scale(1.15); }
  75% { transform: translate(0, 0) scale(1); }
}

@keyframes orbitD3 {
  0%, 100% { transform: translate(0, 0) scale(1); }
  25% { transform: translate(3px, 2px) scale(1.2); }
  50% { transform: translate(-3px, -3px) scale(0.85); }
  75% { transform: translate(2px, -2px) scale(1.1); }
}

.nav-logo:hover .logo-dot.d1 {
  transform: scale(1.4) translateX(-3px);
  box-shadow: 0 0 12px rgba(21, 101, 192, 0.5);
}

.nav-logo:hover .logo-dot.d2 {
  transform: scale(1.5) translateY(-4px);
  box-shadow: 0 0 12px rgba(197, 232, 76, 0.6);
}

.nav-logo:hover .logo-dot.d3 {
  transform: scale(1.6) translateX(3px);
  box-shadow: 0 0 12px rgba(10, 36, 99, 0.4);
}

.nav-links {
  display: flex;
  list-style: none;
  gap: 2rem;
}

.nav-links a {
  color: var(--navy);
  font-size: 0.9rem;
  font-weight: 500;
  letter-spacing: 0.02em;
  position: relative;
  padding: 0.25rem 0;
}

.nav-links a::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--lime);
  transition: width 0.3s ease;
}

.nav-links a:hover::after {
  width: 100%;
}

.nav-academic {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  padding: 0.3rem 0.75rem !important;
  border: 1.5px solid var(--blue-pale);
  border-radius: 20px;
  font-size: 0.82rem !important;
  color: var(--blue) !important;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.nav-academic::after {
  display: none !important;
}

.nav-academic:hover {
  background: var(--blue);
  color: var(--white) !important;
  border-color: var(--blue);
}

.menu-toggle {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
}

.hamburger {
  display: flex;
  flex-direction: column;
  gap: 5px;
  width: 24px;
}

.hamburger span {
  display: block;
  height: 2px;
  background: var(--navy);
  border-radius: 2px;
  transition: all 0.3s ease;
}

.hamburger.open span:nth-child(1) {
  transform: rotate(45deg) translate(5px, 5px);
}

.hamburger.open span:nth-child(2) {
  opacity: 0;
}

.hamburger.open span:nth-child(3) {
  transform: rotate(-45deg) translate(5px, -5px);
}

@media (max-width: 768px) {
  .menu-toggle {
    display: block;
  }

  .nav-links {
    position: fixed;
    top: 0;
    right: -100%;
    width: 70%;
    max-width: 300px;
    height: 100vh;
    background: var(--white);
    flex-direction: column;
    padding: 5rem 2rem 2rem;
    gap: 1.5rem;
    box-shadow: -4px 0 30px rgba(0,0,0,0.1);
    transition: right 0.3s ease;
  }

  .nav-links.open {
    right: 0;
  }

  .nav-links a {
    font-size: 1.1rem;
  }
}
</style>
