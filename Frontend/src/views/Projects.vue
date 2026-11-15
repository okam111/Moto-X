<script setup>
import { ref } from 'vue'

// Import all images from project 1 (supporting jpg, JPG, jpeg, png)
const project1ImageFiles = import.meta.glob('../assets/project 1/*.{jpg,JPG,jpeg,png}', {
  eager: true,
  import: 'default',
})

// Curated interior image metadata for Project 1 (Raptis)
const project1InteriorImages = [
  { src: project1ImageFiles['../assets/project 1/liv1.jpg'], alt: 'Living Area with Aegean Light', size: 'tall' },
  { src: project1ImageFiles['../assets/project 1/k1.jpg'], alt: 'Minimalist Chef Kitchen', size: 'medium' },
  { src: project1ImageFiles['../assets/project 1/master1.jpg'], alt: 'Master Bedroom Suite', size: 'portrait' },
  { src: project1ImageFiles['../assets/project 1/bath1.jpg'], alt: 'Limestone En-Suite Bathroom', size: 'square' },
  { src: project1ImageFiles['../assets/project 1/liv-din-kitch.jpg'], alt: 'Open Plan Living & Dining', size: 'wide' },
  { src: project1ImageFiles['../assets/project 1/master2.jpg'], alt: 'Master Suite Terrace View', size: 'medium' },
  { src: project1ImageFiles['../assets/project 1/bath2.jpg'], alt: 'Bespoke Vanity & Fixtures', size: 'tall' },
  { src: project1ImageFiles['../assets/project 1/liv2.jpg'], alt: 'Lounge Architectural Detail', size: 'square' },
  { src: project1ImageFiles['../assets/project 1/k3.jpg'], alt: 'Kitchen Joinery & Marble Island', size: 'tall' },
  { src: project1ImageFiles['../assets/project 1/bath3.jpg'], alt: 'Walk-in Rain Shower', size: 'medium' },
  { src: project1ImageFiles['../assets/project 1/master3.jpg'], alt: 'Bedroom Dressing Area', size: 'wide' },
  { src: project1ImageFiles['../assets/project 1/kitchen basement.jpg'], alt: 'Secondary Service Kitchen', size: 'square' },
  { src: project1ImageFiles['../assets/project 1/bath4.jpg'], alt: 'Guest Powder Room', size: 'tall' },
  { src: project1ImageFiles['../assets/project 1/living room.jpg'], alt: 'Panoramic Window Framing', size: 'wide' },
  { src: project1ImageFiles['../assets/project 1/bath room.jpg'], alt: 'Bath Sanctuary', size: 'portrait' },
  { src: project1ImageFiles['../assets/project 1/living-din-kitchn.jpg'], alt: 'Fluid Social Pavilion', size: 'medium' },
  { src: project1ImageFiles['../assets/project 1/wc.jpg'], alt: 'Minimalist WC', size: 'square' },
].filter((img) => img.src)

// Exterior images for Project 1 (Raptis)
const project1ExteriorImages = [
  { src: project1ImageFiles['../assets/project 1/1.JPG'], alt: 'Pool & Garden Estate in Daylight', size: 'tall' },
  { src: project1ImageFiles['../assets/project 1/2.JPG'], alt: 'Pool & Garden Illumination at Twilight', size: 'tall' },
  { src: project1ImageFiles['../assets/project 1/3.JPG'], alt: 'Multi-Level Outdoor Lounge & Rooftop Terrace', size: 'square' },
  { src: project1ImageFiles['../assets/project 1/6.JPG'], alt: 'Villa Grounds & Swimming Pool at Sunset', size: 'wide' },
  { src: project1ImageFiles['../assets/project 1/7.JPG'], alt: 'Main Entrance & Front Architectural Facade', size: 'wide' },
  { src: project1ImageFiles['../assets/project 1/8.JPG'], alt: 'Private Driveway & Side Garden at Dusk', size: 'wide' },
  { src: project1ImageFiles['../assets/project 1/9.JPG'], alt: 'Upper Terraces & Minimalist Facade at Sunset', size: 'tall' },
].filter((img) => img.src)

const projects = [
  {
    id: 1,
    number: '01',
    name: 'Raptis',
    subtitle: 'Contemporary Coastal Residence',
    location: 'Porto Rafti, Attica',
    category: 'Private Residence',
    area: '430 m²',
    year: '2025–2026',
    status: 'Work in Progress',
    description:
      'Currently under construction in Porto Rafti, Raptis is a multi-tiered coastal estate sculpted directly into the granite landscape. Designed to fuse indoor tranquility with the Aegean horizon, featuring natural limestone walls, bespoke millwork, and seamless glass facades opening to the sea.',
    interiorImages: project1InteriorImages,
    exteriorImages: project1ExteriorImages,
  },
]

// First project is open by default as requested
const openProjects = ref([0])

// Toggle accordion state
const toggleProject = (index) => {
  const currentIndex = openProjects.value.indexOf(index)
  if (currentIndex > -1) {
    openProjects.value.splice(currentIndex, 1)
  } else {
    openProjects.value.push(index)
  }
}

// Active subtab state for each project: { [projectId]: 'interior' | 'exterior' }
const activeSubtabs = ref({ 1: 'interior' })

const setSubtab = (projectId, tab) => {
  activeSubtabs.value[projectId] = tab
}

// Lightbox modal state
const activeLightboxImage = ref(null)
const openLightbox = (img) => {
  activeLightboxImage.value = img
}
const closeLightbox = () => {
  activeLightboxImage.value = null
}
</script>

<template>
  <div class="projects-page">
    <!-- Header -->
    <header class="page-header">
      <span class="tag">PORTFOLIO · PORTO RAFTI</span>
      <h1 class="page-title">Selected Works</h1>
      <div class="divider"></div>
      <p class="header-desc">
        A curated archive of residential architecture and developments in Porto Rafti, Greece. Each project is an
        exploration of Mediterranean light, geometric stillness, and enduring craftsmanship.
      </p>
    </header>

    <!-- Projects Accordion List -->
    <main class="projects-container">
      <section
        v-for="(project, index) in projects"
        :key="project.id"
        class="project-item"
        :class="{ 'is-expanded': openProjects.includes(index) }"
      >
        <!-- Collapsible Header -->
        <button
          class="project-header"
          @click="toggleProject(index)"
          :aria-expanded="openProjects.includes(index)"
        >
          <div class="header-left">
            <span class="project-num">{{ project.number }}</span>
            <div class="title-group">
              <h2 class="project-name">{{ project.name }}</h2>
              <span class="project-sub">{{ project.subtitle }}</span>
            </div>
          </div>

          <div class="header-right">
            <span
              class="status-badge"
              :class="{ 'status-wip': project.status === 'Work in Progress' }"
            >
              <span v-if="project.status === 'Work in Progress'" class="pulsing-dot"></span>
              {{ project.status }}
            </span>
            <span class="project-loc">{{ project.location }}</span>
            <div class="toggle-indicator">
              <span class="toggle-icon"></span>
            </div>
          </div>
        </button>

        <!-- Collapsible Body Content -->
        <div class="accordion-collapse" :class="{ 'is-open': openProjects.includes(index) }">
          <div class="accordion-body">
            <!-- Project Meta & Brief -->
            <div class="project-info-row">
              <div class="project-description">
                <p>{{ project.description }}</p>
              </div>

              <div class="project-specs">
                <div class="spec-col">
                  <span class="spec-label">TYPE</span>
                  <span class="spec-value">{{ project.category }}</span>
                </div>
                <div class="spec-col">
                  <span class="spec-label">AREA</span>
                  <span class="spec-value">{{ project.area }}</span>
                </div>
                <div class="spec-col">
                  <span class="spec-label">STAGE</span>
                  <span class="spec-value">{{ project.year }}</span>
                </div>
                <div class="spec-col">
                  <span class="spec-label">LOCATION</span>
                  <span class="spec-value">{{ project.location }}</span>
                </div>
              </div>
            </div>

            <!-- Subtabs: Interior / Exterior -->
            <div class="subtabs-bar">
              <button
                class="subtab-btn"
                :class="{ 'is-active': (activeSubtabs[project.id] || 'interior') === 'interior' }"
                @click="setSubtab(project.id, 'interior')"
              >
                <span class="subtab-title">Interior</span>
                <span v-if="project.interiorImages?.length" class="subtab-badge">
                  {{ project.interiorImages.length }}
                </span>
              </button>

              <button
                class="subtab-btn"
                :class="{ 'is-active': activeSubtabs[project.id] === 'exterior' }"
                @click="setSubtab(project.id, 'exterior')"
              >
                <span class="subtab-title">Exterior</span>
                <span v-if="project.exteriorImages?.length" class="subtab-badge">
                  {{ project.exteriorImages.length }}
                </span>
              </button>
            </div>

            <!-- Tab 1: Interior Collage -->
            <div
              v-if="(activeSubtabs[project.id] || 'interior') === 'interior'"
              class="collage-wrapper"
            >
              <div v-if="project.interiorImages?.length" class="collage-masonry">
                <figure
                  v-for="(img, imgIdx) in project.interiorImages"
                  :key="imgIdx"
                  class="collage-item"
                  :class="`size-${img.size || 'medium'}`"
                  @click="openLightbox(img)"
                >
                  <div class="image-inner">
                    <img :src="img.src" :alt="img.alt" loading="lazy" />
                    <div class="overlay">
                      <span class="overlay-text">{{ img.alt }}</span>
                      <span class="overlay-expand">+</span>
                    </div>
                  </div>
                </figure>
              </div>
              <div v-else class="empty-gallery">
                <p>No interior photography available.</p>
              </div>
            </div>

            <!-- Tab 2: Exterior Collage / Placeholder -->
            <div
              v-else-if="activeSubtabs[project.id] === 'exterior'"
              class="collage-wrapper"
            >
              <div v-if="project.exteriorImages?.length" class="collage-masonry">
                <figure
                  v-for="(img, imgIdx) in project.exteriorImages"
                  :key="imgIdx"
                  class="collage-item"
                  :class="`size-${img.size || 'medium'}`"
                  @click="openLightbox(img)"
                >
                  <div class="image-inner">
                    <img :src="img.src" :alt="img.alt" loading="lazy" />
                    <div class="overlay">
                      <span class="overlay-text">{{ img.alt }}</span>
                      <span class="overlay-expand">+</span>
                    </div>
                  </div>
                </figure>
              </div>
              <!-- Placeholder when exterior images are being added -->
              <div v-else class="empty-gallery">
                <span class="empty-tag">EXTERIOR ARCHITECTURE</span>
                <h4 class="empty-heading">Exterior Perspectives & Renders</h4>
                <p class="empty-sub">
                  Exterior captures, private pool terraces, and facade photography will be added here.
                </p>
              </div>
            </div>
          </div>
        </div>
      </section>
    </main>

    <!-- Minimalist Image Lightbox Modal -->
    <Teleport to="body">
      <div v-if="activeLightboxImage" class="lightbox-backdrop" @click="closeLightbox">
        <div class="lightbox-content" @click.stop>
          <button class="lightbox-close" @click="closeLightbox" aria-label="Close image">&times;</button>
          <img :src="activeLightboxImage.src" :alt="activeLightboxImage.alt" />
          <p class="lightbox-caption">{{ activeLightboxImage.alt }}</p>
        </div>
      </div>
    </Teleport>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,400&family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

.projects-page {
  min-height: 100vh;
  background-color: #faf8f5;
  color: #1a1918;
  font-family: 'Montserrat', sans-serif;
  padding-top: 100px;
  padding-bottom: 8rem;
  box-sizing: border-box;
}

/* --- Page Header --- */
.page-header {
  max-width: 1000px;
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

/* --- Projects Container --- */
.projects-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 2rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.project-item {
  background-color: #ffffff;
  border: 1px solid rgba(0, 0, 0, 0.06);
  border-radius: 2px;
  overflow: hidden;
  transition: box-shadow 0.4s ease, border-color 0.4s ease;
}

.project-item:hover {
  border-color: rgba(212, 175, 55, 0.35);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.03);
}

.project-item.is-expanded {
  border-color: rgba(212, 175, 55, 0.4);
  box-shadow: 0 16px 40px rgba(0, 0, 0, 0.05);
}

/* --- Accordion Header Button --- */
.project-header {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 2.2rem 3rem;
  background: none;
  border: none;
  cursor: pointer;
  text-align: left;
  transition: background-color 0.3s ease;
}

.project-header:hover {
  background-color: rgba(245, 243, 239, 0.5);
}

.header-left {
  display: flex;
  align-items: baseline;
  gap: 2rem;
}

.project-num {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 1.6rem;
  font-weight: 400;
  color: #d4af37;
  letter-spacing: 1px;
}

.title-group {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.project-name {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: clamp(1.8rem, 3vw, 2.4rem);
  font-weight: 400;
  color: #171615;
  margin: 0;
  letter-spacing: -0.3px;
}

.project-sub {
  font-size: 0.85rem;
  color: #8c8983;
  font-weight: 400;
  letter-spacing: 0.5px;
}

.header-right {
  display: flex;
  align-items: center;
  gap: 2.5rem;
}

.status-badge {
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  color: #1a1918;
  background-color: #f2eee8;
  padding: 0.4rem 1rem;
  border-radius: 20px;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
}

.status-badge.status-wip {
  background-color: #fdf5e6;
  color: #9c6c18;
  border: 1px solid rgba(212, 175, 55, 0.3);
}

.pulsing-dot {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background-color: #d4af37;
  box-shadow: 0 0 0 0 rgba(212, 175, 55, 0.7);
  animation: pulseGold 2s infinite;
}

@keyframes pulseGold {
  0% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0.7); }
  70% { box-shadow: 0 0 0 6px rgba(212, 175, 55, 0); }
  100% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0); }
}

.project-loc {
  font-size: 0.9rem;
  color: #63605a;
  font-weight: 300;
}

/* Toggle Cross/Minus Indicator */
.toggle-indicator {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  border: 1px solid rgba(212, 175, 55, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  transition: transform 0.4s ease, background-color 0.4s ease;
}

.toggle-icon,
.toggle-icon::after {
  content: '';
  position: absolute;
  background-color: #d4af37;
  transition: transform 0.4s ease;
}

.toggle-icon {
  width: 14px;
  height: 1.5px;
}

.toggle-icon::after {
  width: 1.5px;
  height: 14px;
  top: -6.25px;
  left: 6.25px;
}

.project-item.is-expanded .toggle-icon::after {
  transform: rotate(90deg);
  opacity: 0;
}

.project-item.is-expanded .toggle-indicator {
  background-color: #d4af37;
}

.project-item.is-expanded .toggle-icon {
  background-color: #ffffff;
}

/* --- Accordion Smooth Collapse Animation via CSS Grid --- */
.accordion-collapse {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows 0.6s cubic-bezier(0.16, 1, 0.3, 1), opacity 0.5s ease;
  opacity: 0;
}

.accordion-collapse.is-open {
  grid-template-rows: 1fr;
  opacity: 1;
}

.accordion-body {
  overflow: hidden;
  padding: 0 3rem 3.5rem;
}

/* --- Project Metadata Row --- */
.project-info-row {
  display: grid;
  grid-template-columns: 1.4fr 1fr;
  gap: 3.5rem;
  padding: 2.5rem 0;
  border-top: 1px solid rgba(0, 0, 0, 0.06);
  border-bottom: 1px solid rgba(0, 0, 0, 0.06);
  margin-bottom: 2.5rem;
}

.project-description p {
  font-size: 1.05rem;
  line-height: 1.85;
  color: #55524d;
  font-weight: 300;
  margin: 0;
}

.project-specs {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.8rem;
}

.spec-col {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}

.spec-label {
  font-size: 0.7rem;
  letter-spacing: 2px;
  color: #d4af37;
  font-weight: 600;
}

.spec-value {
  font-size: 0.95rem;
  color: #171615;
  font-weight: 400;
}

/* --- Subtabs Bar (Interior / Exterior) --- */
.subtabs-bar {
  display: flex;
  gap: 2rem;
  border-bottom: 1px solid rgba(0, 0, 0, 0.08);
  margin-bottom: 2.5rem;
}

.subtab-btn {
  background: none;
  border: none;
  border-bottom: 2px solid transparent;
  padding: 0.8rem 0.5rem 1rem;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.9rem;
  font-weight: 500;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: #8c8983;
  transition: all 0.25s ease;
  position: relative;
  top: 1px;
}

.subtab-btn:hover {
  color: #171615;
}

.subtab-btn.is-active {
  color: #d4af37;
  border-bottom-color: #d4af37;
  font-weight: 600;
}

.subtab-badge {
  font-size: 0.7rem;
  background-color: rgba(212, 175, 55, 0.15);
  color: #d4af37;
  padding: 0.15rem 0.5rem;
  border-radius: 12px;
  font-weight: 600;
  letter-spacing: 0;
}

/* --- Collage / Masonry with Varying Vertical Heights --- */
.collage-wrapper {
  width: 100%;
}

.collage-masonry {
  column-count: 3;
  column-gap: 1.8rem;
}

.collage-item {
  margin: 0 0 1.8rem 0;
  break-inside: avoid;
  cursor: pointer;
  overflow: hidden;
  border-radius: 2px;
  background-color: #eeebe6;
  position: relative;
}

.image-inner {
  position: relative;
  width: 100%;
  overflow: hidden;
}

.collage-item img {
  width: 100%;
  display: block;
  object-fit: cover;
  transition: transform 0.7s cubic-bezier(0.16, 1, 0.3, 1), filter 0.4s ease;
}

/* Distinct Vertical Sizing Variations */
.size-tall .image-inner img {
  height: 520px;
}

.size-portrait .image-inner img {
  height: 620px;
}

.size-medium .image-inner img {
  height: 380px;
}

.size-wide .image-inner img {
  height: 270px;
}

.size-square .image-inner img {
  height: 340px;
}

/* Hover Overlay & Zoom */
.overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(20, 19, 18, 0.75) 0%, transparent 60%);
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  padding: 1.5rem;
  opacity: 0;
  transition: opacity 0.4s ease;
}

.overlay-text {
  color: #ffffff;
  font-size: 0.85rem;
  font-weight: 400;
  letter-spacing: 0.5px;
}

.overlay-expand {
  color: #d4af37;
  font-size: 1.3rem;
  font-weight: 300;
}

.collage-item:hover img {
  transform: scale(1.04);
}

.collage-item:hover .overlay {
  opacity: 1;
}

/* Empty Gallery / Placeholder State */
.empty-gallery {
  padding: 5rem 2rem;
  text-align: center;
  background-color: #f7f5f2;
  border: 1px dashed rgba(212, 175, 55, 0.35);
  border-radius: 2px;
  margin: 1rem 0;
}

.empty-tag {
  font-size: 0.7rem;
  letter-spacing: 3px;
  color: #d4af37;
  font-weight: 600;
  text-transform: uppercase;
  display: block;
  margin-bottom: 0.8rem;
}

.empty-heading {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 2rem;
  font-weight: 400;
  color: #171615;
  margin: 0 0 0.8rem 0;
}

.empty-sub {
  font-size: 0.95rem;
  color: #7d7973;
  line-height: 1.7;
  max-width: 520px;
  margin: 0 auto;
  font-weight: 300;
}

/* --- Minimalist Lightbox Modal --- */
.lightbox-backdrop {
  position: fixed;
  inset: 0;
  background-color: rgba(15, 14, 13, 0.92);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  cursor: pointer;
}

.lightbox-content {
  position: relative;
  max-width: 90vw;
  max-height: 90vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: default;
}

.lightbox-content img {
  max-width: 100%;
  max-height: 80vh;
  object-fit: contain;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
  border: 1px solid rgba(212, 175, 55, 0.3);
}

.lightbox-caption {
  margin-top: 1.2rem;
  color: #f0ede8;
  font-size: 0.95rem;
  font-weight: 300;
  letter-spacing: 1px;
}

.lightbox-close {
  position: absolute;
  top: -45px;
  right: -20px;
  background: none;
  border: none;
  color: #ffffff;
  font-size: 2.2rem;
  cursor: pointer;
  line-height: 1;
  transition: color 0.2s ease;
}

.lightbox-close:hover {
  color: #d4af37;
}

/* --- Responsive Adjustments --- */
@media (max-width: 1100px) {
  .collage-masonry {
    column-count: 2;
  }
  .project-info-row {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
}

@media (max-width: 768px) {
  .projects-page {
    padding-top: 80px;
    padding-bottom: 5rem;
  }

  .projects-container {
    padding: 0 1.25rem;
  }

  .project-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 1.2rem;
    padding: 1.8rem 1.5rem;
  }

  .header-left {
    gap: 1.2rem;
  }

  .header-right {
    width: 100%;
    justify-content: space-between;
  }

  .project-loc {
    display: none;
  }

  .accordion-body {
    padding: 0 1.5rem 2rem;
  }

  .subtabs-bar {
    gap: 1.5rem;
    margin-bottom: 2rem;
  }

  .subtab-btn {
    font-size: 0.82rem;
    letter-spacing: 1.5px;
  }

  .collage-masonry {
    column-count: 1;
  }

  .size-tall .image-inner img,
  .size-portrait .image-inner img,
  .size-medium .image-inner img,
  .size-wide .image-inner img,
  .size-square .image-inner img {
    height: 320px;
  }
}

@media (max-width: 480px) {
  .projects-page {
    padding-top: 75px;
  }

  .page-header {
    padding: 2.5rem 1rem 1.5rem;
  }

  .projects-container {
    padding: 0 0.8rem;
    gap: 1.2rem;
  }

  .project-header {
    padding: 1.4rem 1rem;
    gap: 1rem;
  }

  .project-num {
    font-size: 1.3rem;
  }

  .project-name {
    font-size: 1.5rem;
  }

  .project-sub {
    font-size: 0.78rem;
  }

  .status-badge {
    font-size: 0.68rem;
    padding: 0.3rem 0.75rem;
  }

  .toggle-indicator {
    width: 30px;
    height: 30px;
  }

  .accordion-body {
    padding: 0 1rem 1.8rem;
  }

  .project-info-row {
    padding: 1.4rem 0;
    gap: 1.4rem;
    margin-bottom: 1.8rem;
  }

  .project-description p {
    font-size: 0.95rem;
    line-height: 1.7;
  }

  .project-specs {
    gap: 1rem;
  }

  .subtabs-bar {
    gap: 1.2rem;
  }

  .subtab-btn {
    font-size: 0.78rem;
    letter-spacing: 1px;
    padding-bottom: 0.8rem;
  }

  .empty-gallery {
    padding: 3.5rem 1.2rem;
  }

  .empty-heading {
    font-size: 1.6rem;
  }

  .empty-sub {
    font-size: 0.85rem;
  }

  .size-tall .image-inner img,
  .size-portrait .image-inner img,
  .size-medium .image-inner img,
  .size-wide .image-inner img,
  .size-square .image-inner img {
    height: 260px;
  }

  /* Lightbox close button inside viewport */
  .lightbox-close {
    position: fixed;
    top: 15px;
    right: 15px;
    background: rgba(20, 19, 18, 0.7);
    border-radius: 50%;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.8rem;
    z-index: 1002;
  }
}
</style>
