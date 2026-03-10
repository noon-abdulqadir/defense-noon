<script setup>
import { ref } from 'vue'
const imgError = ref(false)
</script>

<template>
  <footer class="footer">
    <div class="footer-dots" aria-hidden="true">
      <span class="fd" style="left:10%; background: var(--blue); width:8px; height:8px; opacity:0.3;" />
      <span class="fd" style="left:25%; background: var(--lime); width:12px; height:12px; opacity:0.2;" />
      <span class="fd" style="left:50%; background: var(--blue-light); width:6px; height:6px; opacity:0.25;" />
      <span class="fd" style="left:75%; background: var(--navy); width:10px; height:10px; opacity:0.15;" />
      <span class="fd" style="left:90%; background: var(--lime); width:8px; height:8px; opacity:0.2;" />
    </div>
    <div class="footer-inner">
      <div class="photographer">
        <div class="photo-avatar-wrap">
          <img
            v-if="!imgError"
            src="https://media.licdn.com/dms/image/v2/D4E03AQHTIO8T-WxgDQ/profile-displayphoto-shrink_200_200/profile-displayphoto-shrink_200_200/0/1696669598795?e=2147483647&v=beta&t=qFivoYs98Ajqy1Sj1oBSFEdkJ6Koq3V169iQaS99mgs"
            alt="Jin Wan"
            class="photo-avatar-img"
            @error="imgError = true"
          />
          <div v-else class="photo-avatar-fallback">JW</div>
        </div>
        <div class="photo-info">
          <span class="photo-label">Photography by</span>
          <span class="photo-name">Jin Wan</span>
        </div>
      </div>

      <div class="footer-divider" />

      <p class="footer-title">
        Noon M.F. Abdulqadir
      </p>
      <p class="footer-text">
        Hiring in the Digital Society
      </p>
      <p class="footer-date">
        9 April 2026 &middot; Amsterdam
      </p>

      <p class="konami-hint" aria-hidden="true">
        &uarr;&uarr;&darr;&darr;&larr;&rarr;&larr;&rarr;BA
      </p>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  padding: 3rem 2rem;
  background: var(--off-white);
  text-align: center;
  position: relative;
  overflow: hidden;
}

.footer-dots {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  display: flex;
}

.fd {
  position: absolute;
  top: -4px;
  border-radius: 50%;
}

.footer-inner {
  max-width: 600px;
  margin: 0 auto;
}

.photographer {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  margin-bottom: 1.5rem;
}

.photo-avatar-wrap {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  overflow: hidden;
  border: 2px solid var(--blue-pale);
  flex-shrink: 0;
}

.photo-avatar-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.photo-avatar-fallback {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--blue-light);
  color: var(--white);
  font-family: 'Playfair Display', serif;
  font-size: 0.9rem;
  font-weight: 600;
}

.photo-info {
  text-align: left;
}

.photo-label {
  display: block;
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--gray-400);
  font-weight: 500;
}

.photo-name {
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--navy);
}

.footer-divider {
  width: 40px;
  height: 2px;
  background: linear-gradient(90deg, var(--blue-pale), var(--lime));
  border-radius: 2px;
  margin: 0 auto 1.5rem;
}

.footer-title {
  font-family: 'Playfair Display', serif;
  font-size: 1.2rem;
  font-weight: 600;
  color: var(--navy);
  margin-bottom: 0.25rem;
}

.footer-text {
  font-size: 0.9rem;
  color: var(--gray-400);
  font-style: italic;
  margin-bottom: 0.25rem;
}

.footer-date {
  font-size: 0.85rem;
  color: var(--gray-400);
}

.konami-hint {
  margin-top: 0.75rem;
  font-size: 0.85rem;
  letter-spacing: 0.3em;
  color: var(--gray-400);
  user-select: none;
}
</style>
