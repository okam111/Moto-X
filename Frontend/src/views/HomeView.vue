<script setup>
import { onMounted, onUnmounted } from 'vue'
import { RouterLink } from 'vue-router'

let observer = null

onMounted(() => {
  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('is-visible')
        }
      })
    },
    { threshold: 0.12, rootMargin: '0px 0px -40px 0px' }
  )

  document.querySelectorAll('.reveal').forEach((el) => {
    observer.observe(el)
  })
})

onUnmounted(() => {
  if (observer) {
    observer.disconnect()
  }
})
</script>

<template>
  <div class="home-view">
    <!-- Hero Section -->
    <div class="hero-container">
      <img src="../assets/Hero.png" id="hero" alt="Porto Rafti Luxury Residence" />
      <div id="hero-text">
        <h1>M O T O</h1>
        <h1>X</h1>
        <h1>Property Developments</h1>
      </div>
      <div class="scroll-indicator">
        <span class="scroll-label">DISCOVER</span>
        <div class="scroll-line"></div>
      </div>
    </div>

    <!-- Body Content -->
    <div class="body">
      <!-- Intro / Mission -->
      <section class="section intro-section reveal">
        <span class="tag">PORTO RAFTI · ATTICA</span>
        <h2 class="editorial-heading">
          Sculpting modern coastal sanctuaries where Aegean tranquility meets architectural purity.
        </h2>
        <div class="divider"></div>
        <p class="lead-text">
          Moto-X is a boutique development studio based in Porto Rafti, Greece. We conceptualize, build,
          and sell contemporary luxury residences tailored for those who value refined geometry, sustainable
          bioclimatic design, and the timeless beauty of the Mediterranean coast.
        </p>
      </section>

      <!-- Highlights / Pillars -->
      <section class="section pillars-section">
        <div class="section-header reveal">
          <span class="tag">CRAFTSMANSHIP & VISION</span>
          <h3 class="editorial-subheading">Built with Intention</h3>
        </div>

        <div class="pillars-grid">
          <div class="pillar-card reveal delay-1">
            <span class="pillar-num">01</span>
            <h4>Prime Coastal Plots</h4>
            <p>
              Strategically selected locations across Porto Rafti that offer panoramic sea vistas, ultimate privacy, and swift access to the coastline.
            </p>
          </div>

          <div class="pillar-card reveal delay-2">
            <span class="pillar-num">02</span>
            <h4>Bioclimatic Architecture</h4>
            <p>
              Sustainably engineered with thermal envelopes, locally sourced limestone, and expansive glass portals that frame the Aegean horizon.
            </p>
          </div>

          <div class="pillar-card reveal delay-3">
            <span class="pillar-num">03</span>
            <h4>Turnkey Excellence</h4>
            <p>
              From concept and excavation to the finest interior finishes, every residence is delivered with rigorous attention to detail and craftsmanship.
            </p>
          </div>
        </div>
      </section>

      <!-- Featured Development Highlight -->
      <section class="section showcase-section reveal">
        <div class="showcase-card">
          <div class="showcase-content">
            <span class="tag">EXCLUSIVE RELEASE</span>
            <h3 class="showcase-title">The Porto Rafti Residences</h3>
            <p class="showcase-desc">
              A private collection of minimalist villas crafted for seamless indoor-outdoor living, private infinity pools, and uninhibited sea breezes.
            </p>

            <div class="showcase-stats">
              <div class="stat-box">
                <span class="stat-value">A+</span>
                <span class="stat-label">Energy Class</span>
              </div>
              <div class="stat-box">
                <span class="stat-value">350m</span>
                <span class="stat-label">To Shoreline</span>
              </div>
              <div class="stat-box">
                <span class="stat-value">2026</span>
                <span class="stat-label">Completion</span>
              </div>
            </div>

            <div class="showcase-action">
              <RouterLink to="/about" class="cta-link">Learn More About Moto-X &rarr;</RouterLink>
            </div>
          </div>
        </div>
      </section>

      <!-- Contact / Inquiries Section -->
      <section class="section contact-section reveal">
        <span class="tag">INQUIRIES</span>
        <h3 class="editorial-heading contact-heading">Begin Your Story in Porto Rafti</h3>
        <p class="lead-text">
          Whether you are looking for an executive residence or a bespoke villa development opportunity, we invite you to connect with our team.
        </p>
        <RouterLink to="/contact" class="primary-btn">Contact Us</RouterLink>
      </section>

      <!-- Minimalist Footer -->
      <footer class="footer">
        <p class="footer-brand">MOTO-X DEVELOPMENT</p>
        <p class="footer-location">Porto Rafti, Attica, Greece</p>
        <p class="footer-copy">&copy; 2026 Moto-X. All rights reserved.</p>
      </footer>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,400&family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

.home-view {
  width: 100%;
  overflow-x: hidden;
  background-color: #faf8f5;
  color: #1e1d1b;
  font-family: 'Montserrat', sans-serif;
}

/* --- Hero Section --- */
.hero-container {
  position: relative;
  width: 100%;
  height: 100dvh;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
}

#hero {
  width: 100%;
  height: 100dvh;
  object-fit: cover;
  object-position: center;
  display: block;
}

#hero-text {
  position: absolute;
  top: 30%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: #ffffff;
  text-shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
  pointer-events: none;
}

#hero-text h1 {
  font-family: 'Montserrat', sans-serif;
  font-weight: 200;
  font-size: clamp(3rem, 7vw, 5.5rem);
  letter-spacing: 12px;
  line-height: 1.15;
}
#hero-text h1:first-of-type {
  font-weight: 400;
}
#hero-text h1:last-of-type {
  font-size: clamp(0.75rem, 1.75vw, 1.375rem);
  font-weight: 300;
}

#hero-text h1:nth-child(2) {
  color: #d4af37;
  text-shadow: #b5b2ac;
}

/* Subtle Scroll Prompt */
.scroll-indicator {
  position: absolute;
  bottom: 2.5rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.6rem;
  color: #ffffff;
  opacity: 0.8;
  pointer-events: none;
}

.scroll-label {
  font-size: 0.65rem;
  letter-spacing: 4px;
  font-weight: 500;
  text-transform: uppercase;
}

.scroll-line {
  width: 1px;
  height: 40px;
  background: linear-gradient(to bottom, rgba(255, 255, 255, 0.8), transparent);
  animation: scrollBounce 2s infinite ease-in-out;
}

@keyframes scrollBounce {
  0% { transform: scaleY(0); transform-origin: top; }
  50% { transform: scaleY(1); transform-origin: top; }
  50.1% { transform: scaleY(1); transform-origin: bottom; }
  100% { transform: scaleY(0); transform-origin: bottom; }
}

/* --- Body Content --- */
.body {
  width: 100%;
  box-sizing: border-box;
}

.section {
  max-width: 1200px;
  margin: 0 auto;
  padding: 7rem 2rem;
  box-sizing: border-box;
}

/* Shared Typography */
.tag {
  display: inline-block;
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: #d4af37;
  margin-bottom: 1.5rem;
}

.editorial-heading {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: clamp(2.1rem, 4.2vw, 3.4rem);
  font-weight: 300;
  line-height: 1.25;
  color: #171615;
  letter-spacing: -0.5px;
}

.editorial-subheading {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: clamp(1.8rem, 3.2vw, 2.6rem);
  font-weight: 300;
  color: #171615;
  margin-top: 0.5rem;
}

.divider {
  width: 50px;
  height: 1px;
  background-color: #d4af37;
  margin: 2.2rem auto;
}

.lead-text {
  font-size: 1.05rem;
  line-height: 1.85;
  font-weight: 300;
  color: #5c5954;
  max-width: 760px;
  margin: 0 auto;
}

/* --- Section 1: Intro --- */
.intro-section {
  text-align: center;
  padding-top: 8rem;
  padding-bottom: 7rem;
}

/* --- Section 2: Pillars Grid --- */
.pillars-section {
  border-top: 1px solid rgba(212, 175, 55, 0.2);
}

.section-header {
  text-align: center;
  margin-bottom: 4rem;
}

.pillars-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2.5rem;
}

.pillar-card {
  background-color: #ffffff;
  padding: 3rem 2.2rem;
  border-radius: 2px;
  border: 1px solid rgba(0, 0, 0, 0.05);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.02);
  transition: transform 0.4s ease, box-shadow 0.4s ease;
}

.pillar-card:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 30px rgba(212, 175, 55, 0.12);
}

.pillar-num {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 1.5rem;
  font-weight: 400;
  color: #d4af37;
  display: block;
  margin-bottom: 1.2rem;
}

.pillar-card h4 {
  font-size: 1.15rem;
  font-weight: 600;
  margin-bottom: 1rem;
  letter-spacing: 0.5px;
  color: #1a1918;
}

.pillar-card p {
  font-size: 0.95rem;
  line-height: 1.75;
  color: #63605a;
  font-weight: 300;
}

/* --- Section 3: Showcase Card --- */
.showcase-section {
  padding-top: 3rem;
  padding-bottom: 6rem;
}

.showcase-card {
  background: linear-gradient(135deg, #1f1e1c 0%, #151413 100%);
  color: #ffffff;
  padding: 4.5rem 3.5rem;
  border-radius: 4px;
  border: 1px solid rgba(212, 175, 55, 0.3);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
}

.showcase-title {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: clamp(2rem, 3.5vw, 2.8rem);
  font-weight: 300;
  margin-bottom: 1.2rem;
  color: #ffffff;
}

.showcase-desc {
  font-size: 1.05rem;
  line-height: 1.8;
  font-weight: 300;
  color: #b5b2ac;
  max-width: 650px;
  margin-bottom: 2.5rem;
}

.showcase-stats {
  display: flex;
  gap: 3.5rem;
  margin-bottom: 3rem;
  padding-top: 1.5rem;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  padding-bottom: 1.5rem;
}

.stat-box {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.stat-value {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 2rem;
  font-weight: 400;
  color: #d4af37;
}

.stat-label {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: #8c8983;
}

.cta-link {
  display: inline-block;
  color: #d4af37;
  text-decoration: none;
  font-size: 0.9rem;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  transition: transform 0.2s ease, opacity 0.2s ease;
}

.cta-link:hover {
  transform: translateX(6px);
  opacity: 0.9;
}

/* --- Section 4: Contact / Inquiry --- */
.contact-section {
  text-align: center;
  padding: 7rem 2rem;
  border-top: 1px solid rgba(212, 175, 55, 0.2);
}

.contact-heading {
  margin-bottom: 1.5rem;
}

.primary-btn {
  display: inline-block;
  margin-top: 2.5rem;
  padding: 1rem 3rem;
  background-color: #1a1918;
  color: #d4af37;
  border: 1px solid #d4af37;
  text-decoration: none;
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 2.5px;
  text-transform: uppercase;
  transition: all 0.3s ease;
}

.primary-btn:hover {
  background-color: #d4af37;
  color: #1a1918;
  box-shadow: 0 8px 25px rgba(212, 175, 55, 0.3);
}

/* --- Footer --- */
.footer {
  background-color: #141312;
  color: #8c8983;
  text-align: center;
  padding: 3.5rem 2rem;
  border-top: 1px solid rgba(212, 175, 55, 0.15);
}

.footer-brand {
  font-size: 0.9rem;
  letter-spacing: 3px;
  font-weight: 600;
  color: #d4af37;
  margin-bottom: 0.4rem;
}

.footer-location {
  font-size: 0.85rem;
  font-weight: 300;
  margin-bottom: 1.2rem;
}

.footer-copy {
  font-size: 0.75rem;
  opacity: 0.6;
}

/* --- Scroll Reveal Animations --- */
.reveal {
  opacity: 0;
  transform: translateY(35px);
  transition: opacity 0.9s cubic-bezier(0.16, 1, 0.3, 1), transform 0.9s cubic-bezier(0.16, 1, 0.3, 1);
  will-change: opacity, transform;
}

.reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}

.delay-1 {
  transition-delay: 0.15s;
}

.delay-2 {
  transition-delay: 0.3s;
}

.delay-3 {
  transition-delay: 0.45s;
}

/* --- Responsive Adjustments --- */
@media (max-width: 900px) {
  .pillars-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .showcase-stats {
    flex-direction: column;
    gap: 1.5rem;
  }

  .showcase-card {
    padding: 3rem 1.8rem;
  }

  .section {
    padding: 5rem 1.5rem;
  }
}
</style>