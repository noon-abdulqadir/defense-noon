<script setup>
import { ref } from 'vue'

const giftOpen = ref(false)

function toggleGift() {
  giftOpen.value = !giftOpen.value
}
</script>

<template>
  <section class="secret-gift-section">
    <div class="secret-gift-inner">
      <button class="gift-toggle" @click="toggleGift" :class="{ open: giftOpen }">
        <span class="gift-warning">NOON DO NOT CLICK ON THIS</span>
        <span class="gift-label">🎁 Secret Gifts & Surprises 🎁</span>
        <svg class="gift-chevron" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><polyline points="6 9 12 15 18 9"/></svg>
      </button>

      <transition name="gift-reveal">
        <div v-if="giftOpen" class="gift-content">

          <div class="gift-block">
            <div class="gift-icon">📖</div>
            <h3>Friendbook</h3>
            <p>We are putting together a memory book for Noon! If you have a favourite photo, memory, or message you'd like to share, please fill out the link below before <strong>April 1st</strong>. We will compile everything into a keepsake book and present it to her at the defense.</p>
            <a href="https://mijnvriendenboekje.nl/nw/index.php?r=participant%2Finvitation-link&bookId=83760&token=FPqJUrOxpvhXyYd4EoHuYtNYli-rm_24XwFJkSmJSkIy" class="gift-link" target="_blank" rel="noopener">
              Fill out the Friendbook
              <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/></svg>
            </a>
          </div>

          <div class="gift-block">
            <div class="gift-icon">✈️</div>
            <h3>Travel Fund</h3>
            <p>After her defense, Noon will be going on a well-deserved trip. We would love to surprise her with a little something to spend along the way!</p>
          </div>

          <div class="gift-block">
            <div class="gift-icon">🤝</div>
            <h3>Donation to Sudan</h3>
            <p>Noon has also asked us to share that she would like to donate half of any contributions received to feed people in Sudan, through <a href="https://sharethemeal.org/campaigns/sudan2" target="_blank" rel="noopener" class="inline-link"><strong>Share the Meal</strong></a>, a UN World Food Programme initiative. Consider that donating <strong>7 euros provides meals for 10 people</strong>.</p>
          </div>

          <div class="gift-block gift-transfer">
            <div class="gift-icon">💸</div>
            <h3>How to Contribute</h3>
            <p>If you would like to contribute to her travel fund and Share the Meal Sudan, you can use either option below. Please add <strong>"PhD Noon + your name"</strong> in the description.</p>

            <div class="pay-options">
              <a href="https://tikkie.me/pay/6q1ff4a45kv0j1isoprk" class="pay-btn tikkie-btn" target="_blank" rel="noopener">
                <img src="/tikkie-logo.png" alt="" class="pay-icon" />
                <span class="pay-brand-name">Contribute via Tikkie</span>
              </a>
              <a href="https://revolut.me/fabiowa78" class="pay-btn revolut-btn" target="_blank" rel="noopener">
                <img src="/revolut-logo.png" alt="" class="pay-icon revolut-icon" />
                <span class="pay-brand-name">Contribute via Revolut</span>
              </a>
            </div>

            <p class="bank-alt">Or transfer directly via bank:</p>
            <div class="gift-account">
              <span class="account-label">Beneficiary</span>
              <span class="account-value">Fabio Votta</span>
              <span class="account-label">IBAN</span>
              <span class="account-value">NL62 REVO 2813 1876 53</span>
              <span class="account-label">BIC / SWIFT</span>
              <span class="account-value">REVONL22</span>
              <span class="account-label">Description</span>
              <span class="account-value">"PhD Noon + your name"</span>
            </div>

            <p class="gift-deadline">Please make your contribution by <strong>Friday, April 3rd</strong>. We will write all names on a card and present it to Noon during the reception.</p>
          </div>

        </div>
      </transition>
    </div>
  </section>
</template>

<style scoped>
.secret-gift-section {
  padding: 2rem 2rem 4rem;
  background: var(--navy);
  position: relative;
}

.secret-gift-inner {
  max-width: 700px;
  margin: 0 auto;
}

.gift-toggle {
  width: 100%;
  background: linear-gradient(135deg, rgba(197, 232, 76, 0.15), rgba(197, 232, 76, 0.05));
  border: 2px dashed rgba(197, 232, 76, 0.4);
  border-radius: 16px;
  padding: 1.25rem 1.5rem;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
  transition: all 0.3s ease;
  font-family: inherit;
  position: relative;
}

.gift-toggle:hover {
  background: linear-gradient(135deg, rgba(197, 232, 76, 0.25), rgba(197, 232, 76, 0.1));
  border-color: rgba(197, 232, 76, 0.6);
  transform: scale(1.01);
}

.gift-warning {
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: #ff6b6b;
  animation: warningPulse 2s ease-in-out infinite;
}

@keyframes warningPulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

.gift-label {
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--lime);
}

.gift-chevron {
  color: var(--lime);
  transition: transform 0.3s ease;
  margin-top: 0.2rem;
}

.gift-toggle.open .gift-chevron {
  transform: rotate(180deg);
}

.gift-content {
  margin-top: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.gift-block {
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 14px;
  padding: 1.5rem;
  text-align: left;
}

.gift-icon {
  font-size: 1.8rem;
  margin-bottom: 0.5rem;
}

.gift-block h3 {
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--lime);
  margin-bottom: 0.75rem;
}

.gift-block p {
  color: var(--blue-pale);
  font-size: 0.95rem;
  line-height: 1.65;
  margin-bottom: 0;
}

.gift-block p strong {
  color: var(--white);
}

.gift-account {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 0.4rem 1rem;
  background: rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  padding: 1rem 1.25rem;
  margin: 1rem 0;
}

.account-label {
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--blue-light);
  font-weight: 500;
}

.account-value {
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--white);
  font-family: 'SF Mono', 'Fira Code', monospace;
}

.pay-options {
  display: flex;
  gap: 1rem;
  margin: 1.25rem 0;
}

.pay-btn {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
  padding: 1rem 1.25rem;
  border-radius: 12px;
  text-decoration: none;
  transition: all 0.3s ease;
  text-align: center;
}

.pay-btn:hover {
  transform: translateY(-2px);
}

.tikkie-btn {
  background: linear-gradient(135deg, #2DC84D, #25B342);
  color: #fff;
}

.tikkie-btn:hover {
  background: linear-gradient(135deg, #38D458, #2DC84D);
  box-shadow: 0 4px 20px rgba(45, 200, 77, 0.35);
}

.revolut-btn {
  background: linear-gradient(135deg, #191C1F, #2D3136);
  color: #fff;
}

.revolut-btn:hover {
  background: linear-gradient(135deg, #2D3136, #3A3F44);
  box-shadow: 0 4px 20px rgba(25, 28, 31, 0.5);
}

.pay-brand-row {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.pay-icon {
  height: 32px;
  width: 32px;
  object-fit: contain;
  border-radius: 8px;
}

.revolut-icon {
  filter: brightness(0) invert(1);
}

.pay-brand-name {
  font-size: 1.3rem;
  font-weight: 800;
  letter-spacing: -0.02em;
}

.pay-btn-label {
  font-weight: 700;
  font-size: 1rem;
}

.pay-btn-desc {
  font-size: 0.78rem;
  opacity: 0.85;
}

.inline-link {
  color: var(--lime);
  text-decoration: underline;
  text-underline-offset: 2px;
  transition: color 0.2s ease;
}

.inline-link:hover {
  color: var(--lime-light);
}

.inline-link strong {
  color: inherit;
}

.bank-alt {
  margin-top: 1rem !important;
  margin-bottom: 0.25rem !important;
  font-size: 0.85rem !important;
  color: var(--blue-light) !important;
  text-align: center;
}

@media (max-width: 480px) {
  .pay-options {
    flex-direction: column;
  }
}

.gift-deadline {
  margin-top: 0.75rem !important;
  font-size: 0.88rem !important;
  font-style: italic;
}

.gift-link {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  margin-top: 1rem;
  padding: 0.7rem 1.4rem;
  background: var(--lime);
  color: var(--navy);
  font-weight: 700;
  font-size: 0.9rem;
  border-radius: 50px;
  transition: all 0.3s ease;
  text-decoration: none;
}

.gift-link:hover {
  background: var(--lime-light);
  transform: translateY(-2px);
  box-shadow: 0 4px 20px rgba(197, 232, 76, 0.3);
}

.gift-reveal-enter-active {
  transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1);
}

.gift-reveal-leave-active {
  transition: all 0.3s ease-in;
}

.gift-reveal-enter-from {
  opacity: 0;
  transform: translateY(-10px);
}

.gift-reveal-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
</style>
