<script setup>
import { RouterLink } from 'vue-router'
</script>

<template>
  <nav class="nav-bar">
    <div class="brand">
      <RouterLink to="/" class="brand-link">Moto-X Development</RouterLink>
    </div>
    <div class="nav-links">
      <RouterLink to="/" class="link">Home</RouterLink>
      <RouterLink to="/projects" class="link">Projects</RouterLink>
      <RouterLink to="/about" class="link">About</RouterLink>
      <RouterLink to="/contact" class="link">Contact</RouterLink>
    </div>
  </nav>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

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
  background-color: rgba(235, 232, 230, 0.717);
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
}

.brand-link:hover {
  opacity: 0.8;
}

.nav-links {
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

/* Elegant gold underline for the active page */
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

@media (max-width: 768px) {
  .nav-bar {
    padding: 0 1.5rem;
  }
  .nav-links {
    gap: 1.25rem;
  }
  .brand-link {
    font-size: 1rem;
  }
}
</style>
