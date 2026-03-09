<script setup>
import HeroSection from './components/HeroSection.vue'
import AboutSection from './components/AboutSection.vue'
import TimelineSection from './components/TimelineSection.vue'
import VenueSection from './components/VenueSection.vue'
import ParanymphsSection from './components/ParanymphsSection.vue'
import PartySection from './components/PartySection.vue'
import FooterSection from './components/FooterSection.vue'
import NavBar from './components/NavBar.vue'
import KonamiDance from './components/KonamiDance.vue'
import SecretGift from './components/SecretGift.vue'
</script>

<template>
  <NavBar />
  <KonamiDance />
  <main>
    <HeroSection />
    <SecretGift />
    <TimelineSection />
    <VenueSection />
    <ParanymphsSection />
    <PartySection />
    <AboutSection />
    <FooterSection />
  </main>
</template>

<style scoped>
main {
  width: 100%;
}
</style>
