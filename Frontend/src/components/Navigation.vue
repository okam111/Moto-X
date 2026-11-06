<script setup>
import { ref, watch } from 'vue'
import { RouterLink, useRoute } from 'vue-router'

const route = useRoute()
const isMobileMenuOpen = ref(false)

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
  if (isMobileMenuOpen.value) {
    document.body.style.overflow = 'hidden'
  } else {
    document.body.style.overflow = ''
  }
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
  document.body.style.overflow = ''
}

// Automatically close menu when route changes
watch(
  () => route.path,
  () => {
    closeMobileMenu()
  }
)
</script>

<template>
  <nav class="nav-bar">
    <div class="brand">
      <RouterLink to="/" class="brand-link" @click="closeMobileMenu">
        Moto-X Development
      </RouterLink>
    </div>

    <!-- Desktop Navigation Links -->
    <div class="nav-links desktop-links">
      <RouterLink to="/" class="link">Home</RouterLink>
      <RouterLink to="/projects" class="link">Projects</RouterLink>
      <RouterLink to="/about" class="link">About</RouterLink>
      <RouterLink to="/contact" class="link">Contact</RouterLink>
    </div>

    <!-- Mobile Hamburger Toggle Button -->
    <button
      class="hamburger-btn"
      :class="{ 'is-active': isMobileMenuOpen }"
      @click="toggleMobileMenu"
      aria-label="Toggle navigation menu"
      :aria-expanded="isMobileMenuOpen"
    >
      <span class="bar bar-1"></span>
      <span class="bar bar-2"></span>
      <span class="bar bar-3"></span>
    </button>

    <!-- Mobile Menu Drawer Overlay -->
    <Teleport to="body">
      <div
        class="mobile-menu-overlay"
        :class="{ 'is-open': isMobileMenuOpen }"
        @click="closeMobileMenu"
      >
        <div class="mobile-menu-panel" @click.stop>
          <div class="mobile-menu-header">
            <span class="mobile-brand-tag">MOTO-X · PORTO RAFTI</span>
            <button
              class="mobile-close-btn"
              @click="closeMobileMenu"
              aria-label="Close menu"
            >
              &times;
            </button>
          </div>

          <div class="mobile-links-list">
            <RouterLink to="/" class="mobile-link" @click="closeMobileMenu">
              <span class="link-num">01</span>
              <span class="link-text">Home</span>
            </RouterLink>
            <RouterLink to="/projects" class="mobile-link" @click="closeMobileMenu">
              <span class="link-num">02</span>
              <span class="link-text">Projects</span>
            </RouterLink>
            <RouterLink to="/about" class="mobile-link" @click="closeMobileMenu">
              <span class="link-num">03</span>
              <span class="link-text">About</span>
            </RouterLink>
            <RouterLink to="/contact" class="mobile-link" @click="closeMobileMenu">
              <span class="link-num">04</span>
              <span class="link-text">Contact</span>
            </RouterLink>
          </div>

          <div class="mobile-menu-footer">
            <p class="mobile-contact-line">Krionerio 11, Porto Rafti 19003</p>
            <a href="tel:+306986896903" class="mobile-phone-link">+30 698 6896903</a>
          </div>
        </div>
      </div>
    </Teleport>
  </nav>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,400&family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

.nav-bar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 70px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 3rem;
  box-sizing: border-box;
  background-color: rgba(235, 232, 230, 0.75);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border-bottom: 1px solid rgba(212, 175, 55, 0.2);
  z-index: 100;
  font-family: 'Montserrat', sans-serif;
}

.brand-link {
  text-decoration: none;
  color: #d4af37;
  font-size: 1.15rem;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  transition: opacity 0.2s ease;
  white-space: nowrap;
}

.brand-link:hover {
  opacity: 0.8;
}

/* Desktop Links */
.desktop-links {
  display: flex;
  align-items: center;
  gap: 2.5rem;
}

.link {
  text-decoration: none;
  color: #d4af37;
  font-size: 0.95rem;
  font-weight: 500;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  position: relative;
  padding: 0.25rem 0;
  transition: color 0.2s ease, opacity 0.2s ease;
}

.link:hover {
  opacity: 0.8;
}

.link.router-link-active::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: #d4af37;
  border-radius: 1px;
}

/* Hamburger Toggle Button */
.hamburger-btn {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
  width: 44px;
  height: 44px;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 5px;
  z-index: 102;
}

.bar {
  display: block;
  width: 22px;
  height: 2px;
  background-color: #d4af37;
  transition: transform 0.3s ease, opacity 0.3s ease;
  border-radius: 1px;
}

.hamburger-btn.is-active .bar-1 {
  transform: translateY(7px) rotate(45deg);
}

.hamburger-btn.is-active .bar-2 {
  opacity: 0;
}

.hamburger-btn.is-active .bar-3 {
  transform: translateY(-7px) rotate(-45deg);
}

/* Mobile Menu Overlay Drawer */
.mobile-menu-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(12, 11, 10, 0.7);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  z-index: 1500;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.35s ease;
}

.mobile-menu-overlay.is-open {
  opacity: 1;
  pointer-events: auto;
}

.mobile-menu-panel {
  position: absolute;
  top: 0;
  right: 0;
  width: 82vw;
  max-width: 360px;
  height: 100vh;
  height: 100dvh;
  background: #171615;
  border-left: 1px solid rgba(212, 175, 55, 0.25);
  box-shadow: -10px 0 30px rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 2.5rem 2rem;
  box-sizing: border-box;
  transform: translateX(100%);
  transition: transform 0.35s cubic-bezier(0.16, 1, 0.3, 1);
}

.mobile-menu-overlay.is-open .mobile-menu-panel {
  transform: translateX(0);
}

.mobile-menu-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  padding-bottom: 1.2rem;
}

.mobile-brand-tag {
  font-size: 0.7rem;
  letter-spacing: 2.5px;
  color: #d4af37;
  font-weight: 600;
}

.mobile-close-btn {
  background: none;
  border: none;
  color: #ffffff;
  font-size: 2rem;
  line-height: 1;
  cursor: pointer;
  padding: 0.2rem 0.5rem;
  transition: color 0.2s ease;
}

.mobile-close-btn:hover {
  color: #d4af37;
}

.mobile-links-list {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  margin: auto 0;
}

.mobile-link {
  display: flex;
  align-items: baseline;
  gap: 1.2rem;
  text-decoration: none;
  color: #f5f2eb;
  padding: 0.6rem 0;
  transition: all 0.2s ease;
  border-bottom: 1px solid rgba(255, 255, 255, 0.04);
}

.link-num {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 1.1rem;
  color: #d4af37;
  font-weight: 400;
  letter-spacing: 1px;
}

.link-text {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 2rem;
  font-weight: 300;
  letter-spacing: 0.5px;
}

.mobile-link.router-link-active .link-text {
  color: #d4af37;
  font-weight: 400;
}

.mobile-link:active {
  opacity: 0.7;
}

.mobile-menu-footer {
  border-top: 1px solid rgba(255, 255, 255, 0.08);
  padding-top: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.mobile-contact-line {
  font-size: 0.75rem;
  color: #8c8983;
  margin: 0;
  font-weight: 300;
}

.mobile-phone-link {
  font-size: 0.95rem;
  color: #d4af37;
  text-decoration: none;
  font-weight: 500;
}

/* Tablet & Mobile Breakpoints */
@media (max-width: 860px) {
  .nav-bar {
    padding: 0 1.5rem;
    height: 64px;
  }

  .desktop-links {
    display: none;
  }

  .hamburger-btn {
    display: flex;
  }

  .brand-link {
    font-size: 0.95rem;
    letter-spacing: 1.5px;
  }
}

@media (max-width: 400px) {
  .nav-bar {
    padding: 0 1rem;
  }

  .brand-link {
    font-size: 0.85rem;
    letter-spacing: 1px;
  }
}
</style>
