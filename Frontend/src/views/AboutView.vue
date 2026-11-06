<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <div class="about-page">
    <!-- Header -->
    <header class="page-header">
      <span class="tag">STUDIO PROFILE · PORTO RAFTI</span>
      <h1 class="page-title">The Vision of Moto-X</h1>
      <div class="divider"></div>
      <p class="header-desc">
        Pioneering contemporary architectural craft and bespoke coastal residences across the Attica riviera.
        We build homes shaped by Aegean clarity, native stone, and timeless geometric stillness.
      </p>
    </header>

    <!-- Main Content -->
    <main class="about-content">
      <!-- Section 1: Editorial Introduction -->
      <section class="story-section">
        <div class="story-grid">
          <div class="story-text">
            <span class="sub-tag">OUR ORIGIN</span>
            <h2 class="editorial-heading">Rooted in the Mediterranean Coastline</h2>
            <p>
              Headquartered at <strong>Krionerio 11 in Porto Rafti</strong>, Moto-X was founded with a singular conviction:
              that modern residences should exist in quiet dialogue with their surrounding topography.
            </p>
            <p>
              Porto Rafti offers one of the most serene coastal environments in Attica—sheltered crystalline bays,
              rugged granite slopes, and pine-clad hills, just 15 minutes from Athens International Airport. We leverage
              this unique setting to develop private residences and bespoke villas that capture the spirit of elevated
              Mediterranean living.
            </p>
          </div>

          <div class="story-card">
            <div class="card-inner">
              <span class="card-num">100%</span>
              <span class="card-title">Dedicated to Porto Rafti</span>
              <p class="card-desc">
                Every project is intimately connected to local soil, climate patterns, and coastal views, engineered to
                thrive in harmony with the Aegean elements.
              </p>
            </div>
          </div>
        </div>
      </section>

      <!-- Section 2: Core Philosophy Pillars -->
      <section class="values-section">
        <div class="values-header">
          <span class="sub-tag">METHODOLOGY</span>
          <h2 class="editorial-heading">The Pillars of Our Practice</h2>
        </div>

        <div class="values-grid">
          <div class="value-card">
            <span class="value-num">01</span>
            <h3>Topography & Orientation</h3>
            <p>
              We design in direct conversation with the land. Every villa is carved to maximize panoramic sea vistas,
              harness prevailing maritime breezes, and shield living pavilions from intense summer sun.
            </p>
          </div>

          <div class="value-card">
            <span class="value-num">02</span>
            <h3>Material Honesty</h3>
            <p>
              Our palettes celebrate local Greek limestone, honed micro-cement, warm smoked timber, and expansive
              apertures of thermal glass. Finishes that age gracefully under the coastal sun.
            </p>
          </div>

          <div class="value-card">
            <span class="value-num">03</span>
            <h3>Turnkey Mastery</h3>
            <p>
              We oversee the complete lifecycle of each estate—from land acquisition and zoning permits to structural
              engineering, bespoke millwork, and private key handover.
            </p>
          </div>
        </div>
      </section>

      <!-- Section 3: Call to Action -->
      <section class="cta-banner">
        <div class="cta-inner">
          <span class="sub-tag">COLLABORATE</span>
          <h2 class="editorial-heading cta-title">Discover Your Next Residence</h2>
          <p class="cta-sub">
            Explore our active developments or consult with our project directors regarding private acquisitions in
            Porto Rafti.
          </p>
          <div class="cta-buttons">
            <RouterLink to="/projects" class="primary-btn">View Selected Works</RouterLink>
            <RouterLink to="/contact" class="secondary-btn">Contact Studio &rarr;</RouterLink>
          </div>
        </div>
      </section>
    </main>

    <!-- Footer -->
    <footer class="footer">
      <p class="footer-brand">MOTO-X DEVELOPMENT</p>
      <p class="footer-location">Krionerio 11, Porto Rafti 19003, Greece</p>
      <p class="footer-copy">&copy; 2026 Moto-X. All rights reserved.</p>
    </footer>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,400&family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

.about-page {
  min-height: 100vh;
  background-color: #faf8f5;
  color: #1a1918;
  font-family: 'Montserrat', sans-serif;
  padding-top: 100px;
  box-sizing: border-box;
}

/* Page Header */
.page-header {
  max-width: 900px;
  margin: 0 auto;
  padding: 4rem 2rem 3rem;
  text-align: center;
}

.tag {
  display: inline-block;
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: #d4af37;
  margin-bottom: 1.2rem;
}

.sub-tag {
  font-size: 0.7rem;
  letter-spacing: 2.5px;
  text-transform: uppercase;
  color: #d4af37;
  font-weight: 600;
  display: block;
  margin-bottom: 0.6rem;
}

.page-title {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: clamp(2.5rem, 5vw, 4.2rem);
  font-weight: 300;
  letter-spacing: -0.5px;
  color: #151413;
  margin: 0;
}

.divider {
  width: 50px;
  height: 1px;
  background-color: #d4af37;
  margin: 2rem auto;
}

.header-desc {
  font-size: 1.05rem;
  line-height: 1.85;
  font-weight: 300;
  color: #63605a;
  max-width: 680px;
  margin: 0 auto;
}

/* Main Content */
.about-content {
  max-width: 1240px;
  margin: 0 auto 6rem;
  padding: 0 2rem;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  gap: 6rem;
}

/* Story Section */
.story-grid {
  display: grid;
  grid-template-columns: 1.4fr 1fr;
  gap: 4rem;
  align-items: center;
}

.editorial-heading {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: clamp(2rem, 3.8vw, 2.9rem);
  font-weight: 300;
  color: #171615;
  margin: 0 0 1.5rem 0;
  line-height: 1.25;
}

.story-text p {
  font-size: 1.05rem;
  line-height: 1.85;
  color: #55524d;
  font-weight: 300;
  margin-bottom: 1.2rem;
}

.story-card {
  background: linear-gradient(135deg, #1f1e1c 0%, #151413 100%);
  border: 1px solid rgba(212, 175, 55, 0.35);
  border-radius: 3px;
  padding: 3rem 2.5rem;
  color: #ffffff;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.15);
}

.card-num {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 3.5rem;
  font-weight: 300;
  color: #d4af37;
  display: block;
  line-height: 1;
  margin-bottom: 0.8rem;
}

.card-title {
  font-size: 1.15rem;
  font-weight: 500;
  color: #ffffff;
  display: block;
  margin-bottom: 0.8rem;
}

.card-desc {
  font-size: 0.95rem;
  line-height: 1.75;
  color: #aba69e;
  margin: 0;
  font-weight: 300;
}

/* Values Grid */
.values-header {
  text-align: center;
  margin-bottom: 3.5rem;
}

.values-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2.2rem;
}

.value-card {
  background-color: #ffffff;
  padding: 3rem 2.2rem;
  border: 1px solid rgba(0, 0, 0, 0.06);
  border-radius: 2px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.value-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 12px 30px rgba(212, 175, 55, 0.12);
}

.value-num {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 1.5rem;
  color: #d4af37;
  display: block;
  margin-bottom: 1.2rem;
}

.value-card h3 {
  font-size: 1.15rem;
  font-weight: 600;
  color: #171615;
  margin: 0 0 1rem 0;
}

.value-card p {
  font-size: 0.95rem;
  line-height: 1.75;
  color: #63605a;
  font-weight: 300;
  margin: 0;
}

/* CTA Banner */
.cta-banner {
  background-color: #ffffff;
  border: 1px solid rgba(212, 175, 55, 0.3);
  padding: 4.5rem 3rem;
  border-radius: 2px;
  text-align: center;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.02);
}

.cta-title {
  margin-bottom: 1rem;
}

.cta-sub {
  font-size: 1.05rem;
  line-height: 1.8;
  color: #63605a;
  max-width: 620px;
  margin: 0 auto 2.5rem;
  font-weight: 300;
}

.cta-buttons {
  display: flex;
  justify-content: center;
  gap: 1.5rem;
  flex-wrap: wrap;
}

.primary-btn {
  background-color: #171615;
  color: #d4af37;
  border: 1px solid #d4af37;
  padding: 1rem 2.5rem;
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  text-decoration: none;
  transition: all 0.3s ease;
}

.primary-btn:hover {
  background-color: #d4af37;
  color: #171615;
  box-shadow: 0 8px 25px rgba(212, 175, 55, 0.25);
}

.secondary-btn {
  background: none;
  color: #171615;
  border: 1px solid rgba(0, 0, 0, 0.2);
  padding: 1rem 2.5rem;
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  text-decoration: none;
  transition: all 0.3s ease;
}

.secondary-btn:hover {
  border-color: #d4af37;
  color: #d4af37;
}

/* Footer */
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

/* Responsive Adjustments */
@media (max-width: 992px) {
  .story-grid {
    grid-template-columns: 1fr;
    gap: 2.5rem;
  }
  .values-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }
  .about-content {
    gap: 4.5rem;
  }
}

@media (max-width: 600px) {
  .about-page {
    padding-top: 75px;
  }

  .page-header {
    padding: 2.5rem 1rem 1.5rem;
  }

  .about-content {
    padding: 0 1rem;
    gap: 3.5rem;
    margin-bottom: 4rem;
  }

  .story-card {
    padding: 2.2rem 1.4rem;
  }

  .card-num {
    font-size: 2.8rem;
  }

  .value-card {
    padding: 2rem 1.3rem;
  }

  .cta-banner {
    padding: 2.8rem 1.4rem;
  }

  .cta-buttons {
    flex-direction: column;
    width: 100%;
  }

  .primary-btn,
  .secondary-btn {
    width: 100%;
    text-align: center;
    box-sizing: border-box;
  }

  .footer {
    padding: 2.5rem 1.2rem;
  }
}
</style>
