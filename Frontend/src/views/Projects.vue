<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

// Import all images from project 1
const project1ImageFiles = import.meta.glob('../assets/project 1/*.jpg', {
  eager: true,
  import: 'default',
})

// Curated image metadata for Project 1 with varying vertical height classes
const project1Images = [
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

// Defined projects list
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
    hasWalkthrough: true,
    description:
      'Currently under construction in Porto Rafti, Raptis is a multi-tiered coastal estate sculpted directly into the granite landscape. Designed to fuse indoor tranquility with the Aegean horizon, featuring natural limestone walls, bespoke millwork, and seamless glass facades opening to the sea.',
    images: project1Images,
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

// Lightbox modal state
const activeLightboxImage = ref(null)
const openLightbox = (img) => {
  activeLightboxImage.value = img
}
const closeLightbox = () => {
  activeLightboxImage.value = null
}

// --- Interactive Spatial Walkthrough State ---
const isWalkthroughOpen = ref(false)
const currentStepIndex = ref(0)

// Spatial house tour graph with directional connections between rooms
const walkthroughSteps = [
  {
    id: 'living',
    name: 'Main Living Salon',
    level: 'Ground Level · Central Wing',
    image: project1ImageFiles['../assets/project 1/liv1.jpg'],
    description:
      'Enter through the main panoramic foyer into the sunken living salon. Floor-to-ceiling glass captures the changing Aegean daylight throughout the afternoon.',
    actions: [
      { label: 'Walk to Dining & Kitchen →', targetId: 'dining' },
      { label: 'Examine Salon Windows & View →', targetId: 'living-detail' },
    ],
  },
  {
    id: 'living-detail',
    name: 'Living Room Panorama',
    level: 'Ground Level · South Porch',
    image: project1ImageFiles['../assets/project 1/living room.jpg'],
    description:
      'Framed architectural apertures orienting towards the Porto Rafti bay. Micro-cement floors flow seamlessly outward toward the exterior deck.',
    actions: [
      { label: '← Back to Main Living Salon', targetId: 'living' },
      { label: 'Continue to Dining Area →', targetId: 'dining' },
    ],
  },
  {
    id: 'dining',
    name: 'Dining & Social Pavilion',
    level: 'Ground Level · Dining Core',
    image: project1ImageFiles['../assets/project 1/liv-din-kitch.jpg'],
    description:
      'The open-concept dining space connects the social lounge with the culinary studio, allowing for seamless entertaining.',
    actions: [
      { label: '← Back to Living Salon', targetId: 'living' },
      { label: 'Step into Chef’s Kitchen →', targetId: 'kitchen' },
      { label: 'Go Upstairs to Master Suite →', targetId: 'master-dressing' },
    ],
  },
  {
    id: 'kitchen',
    name: 'Chef’s Kitchen',
    level: 'Ground Level · Culinary Studio',
    image: project1ImageFiles['../assets/project 1/k1.jpg'],
    description:
      'Monolithic quartz island with integrated induction cooktop and discreet flush exhaust, wrapped in matte architectural cabinetry.',
    actions: [
      { label: '← Back to Dining Pavilion', targetId: 'dining' },
      { label: 'Inspect Cabinetry & Joinery →', targetId: 'kitchen-detail' },
      { label: 'Descend to Lower Prep Kitchen ↓', targetId: 'basement-kitchen' },
      { label: 'Head Upstairs to Bedrooms ↑', targetId: 'master-dressing' },
    ],
  },
  {
    id: 'kitchen-detail',
    name: 'Kitchen Joinery & Breakfast Bar',
    level: 'Ground Level · East Wing',
    image: project1ImageFiles['../assets/project 1/k3.jpg'],
    description:
      'Concealed appliances, built-in wine cooling, warm LED channel lighting, and fluted acoustic wood paneling.',
    actions: [
      { label: '← Back to Kitchen Island', targetId: 'kitchen' },
      { label: 'Take Stairs Upstairs →', targetId: 'master-dressing' },
    ],
  },
  {
    id: 'basement-kitchen',
    name: 'Secondary Service Kitchen',
    level: 'Lower Level · Service Quarters',
    image: project1ImageFiles['../assets/project 1/kitchen basement.jpg'],
    description:
      'An auxiliary culinary workspace equipped for private catering and extensive event preparation, with direct rear service access.',
    actions: [
      { label: '↑ Return Upstairs to Main Kitchen', targetId: 'kitchen' },
    ],
  },
  {
    id: 'master-dressing',
    name: 'Master Suite Dressing Vestibule',
    level: 'Upper Level · Private Gallery',
    image: project1ImageFiles['../assets/project 1/master3.jpg'],
    description:
      'Ascending the floating timber stairs leads to the private master wing, featuring custom full-height smoked oak wardrobes.',
    actions: [
      { label: '↓ Return Downstairs to Dining', targetId: 'dining' },
      { label: 'Enter Master Bedroom →', targetId: 'master-bed' },
    ],
  },
  {
    id: 'master-bed',
    name: 'Master Bedroom Suite',
    level: 'Upper Level · Primary Suite',
    image: project1ImageFiles['../assets/project 1/master1.jpg'],
    description:
      'Positioned to face the morning sun rising over the Porto Rafti coves. Acoustic wall paneling and motorized privacy sheers provide ultimate seclusion.',
    actions: [
      { label: '← Back to Dressing Vestibule', targetId: 'master-dressing' },
      { label: 'Step onto Private Terrace →', targetId: 'master-terrace' },
      { label: 'Enter En-Suite Sanctuary →', targetId: 'master-bath' },
    ],
  },
  {
    id: 'master-terrace',
    name: 'Master Private Terrace',
    level: 'Upper Level · Sea Balcony',
    image: project1ImageFiles['../assets/project 1/master2.jpg'],
    description:
      'Cantilevered private terrace with frameless glass balustrades offering sweeping uninterrupted panoramas of the Aegean shoreline.',
    actions: [
      { label: '← Return to Master Bedroom', targetId: 'master-bed' },
      { label: 'Proceed to En-Suite Bath →', targetId: 'master-bath' },
    ],
  },
  {
    id: 'master-bath',
    name: 'Master En-Suite Sanctuary',
    level: 'Upper Level · Wet Suite',
    image: project1ImageFiles['../assets/project 1/bath1.jpg'],
    description:
      'Curated Greek limestone cladding with a deep freestanding soaking tub positioned toward the tranquil mountain and sea views.',
    actions: [
      { label: '← Back to Master Bedroom', targetId: 'master-bed' },
      { label: 'View Limestone Vanity →', targetId: 'master-vanity' },
      { label: 'Enter Rain Shower Enclosure →', targetId: 'master-shower' },
    ],
  },
  {
    id: 'master-vanity',
    name: 'Bespoke Double Vanity',
    level: 'Upper Level · Bath Wing',
    image: project1ImageFiles['../assets/project 1/bath2.jpg'],
    description:
      'Dual carved limestone sinks with wall-mounted brushed bronze fixtures and halo ambient lighting.',
    actions: [
      { label: '← Back to Soaking Tub', targetId: 'master-bath' },
      { label: 'Walk into Rain Shower →', targetId: 'master-shower' },
    ],
  },
  {
    id: 'master-shower',
    name: 'Walk-in Rain Shower & Wet Room',
    level: 'Upper Level · Bath Enclosure',
    image: project1ImageFiles['../assets/project 1/bath3.jpg'],
    description:
      'Seamless curbless rain shower room featuring integrated bench seating and dual thermostatic shower controls.',
    actions: [
      { label: '← Back to Vanity', targetId: 'master-vanity' },
      { label: 'Visit Guest Facilities →', targetId: 'guest-bath' },
      { label: '← Return to Bedroom', targetId: 'master-bed' },
    ],
  },
  {
    id: 'guest-bath',
    name: 'Guest Bathroom',
    level: 'Ground Level · Guest Wing',
    image: project1ImageFiles['../assets/project 1/bath4.jpg'],
    description:
      'Refined guest bathroom featuring textured beige stoneware, floating vanity, and walk-in glass shower.',
    actions: [
      { label: '↑ Back Upstairs to Master Suite', targetId: 'master-bed' },
      { label: 'View Guest Powder Room →', targetId: 'powder-room' },
      { label: 'Return to Main Living Salon →', targetId: 'living' },
    ],
  },
  {
    id: 'powder-room',
    name: 'Guest Powder Room',
    level: 'Ground Level · Entry Corridor',
    image: project1ImageFiles['../assets/project 1/wc.jpg'],
    description:
      'Intimate powder room with minimalist wall-hung toilet, discrete soft downlighting, and matte bronze detailing.',
    actions: [
      { label: '← Back to Guest Bathroom', targetId: 'guest-bath' },
      { label: 'Finish Tour & Return to Living Salon →', targetId: 'living' },
    ],
  },
].filter((step) => step.image)

const currentStep = computed(() => walkthroughSteps[currentStepIndex.value] || walkthroughSteps[0])

const startWalkthrough = () => {
  currentStepIndex.value = 0
  isWalkthroughOpen.value = true
}

const closeWalkthrough = () => {
  isWalkthroughOpen.value = false
}

const nextStep = () => {
  if (currentStepIndex.value < walkthroughSteps.length - 1) {
    currentStepIndex.value++
  } else {
    currentStepIndex.value = 0
  }
}

const prevStep = () => {
  if (currentStepIndex.value > 0) {
    currentStepIndex.value--
  } else {
    currentStepIndex.value = walkthroughSteps.length - 1
  }
}

const navigateToRoom = (targetId) => {
  const index = walkthroughSteps.findIndex((step) => step.id === targetId)
  if (index !== -1) {
    currentStepIndex.value = index
  }
}

// Keyboard shortcuts for walkthrough navigation
const handleKeyDown = (e) => {
  if (!isWalkthroughOpen.value) return
  if (e.key === 'ArrowRight') nextStep()
  if (e.key === 'ArrowLeft') prevStep()
  if (e.key === 'Escape') closeWalkthrough()
}

onMounted(() => {
  window.addEventListener('keydown', handleKeyDown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeyDown)
})
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

                <!-- Walkthrough CTA Banner for Raptis -->
                <div v-if="project.hasWalkthrough" class="walkthrough-cta-box">
                  <div class="cta-text">
                    <span class="cta-badge">INTERACTIVE EXPERIENCE</span>
                    <h3 class="cta-title">Explore the Residence Step-by-Step</h3>
                    <p class="cta-desc">
                      Take a spatial room-by-room walkthrough of Raptis with interactive advancement buttons to move between the living areas, kitchen, and private master suite.
                    </p>
                  </div>
                  <button class="walkthrough-btn" @click="startWalkthrough">
                    <span class="btn-icon">✦</span> Start House Walkthrough
                  </button>
                </div>
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

            <!-- Collage of Images with Varying Vertical Heights -->
            <div class="collage-wrapper">
              <div class="collage-masonry">
                <figure
                  v-for="(img, imgIdx) in project.images"
                  :key="imgIdx"
                  class="collage-item"
                  :class="`size-${img.size}`"
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
            </div>
          </div>
        </div>
      </section>
    </main>

    <!-- Interactive House Walkthrough Modal -->
    <Teleport to="body">
      <div v-if="isWalkthroughOpen" class="walkthrough-overlay">
        <div class="walkthrough-container">
          <!-- Top Control Bar -->
          <header class="tour-top-bar">
            <div class="tour-branding">
              <span class="tour-brand-name">RAPTIS</span>
              <span class="tour-brand-sep">/</span>
              <span class="tour-brand-sub">PORTO RAFTI WALKTHROUGH</span>
            </div>

            <!-- Step Progress Counter -->
            <div class="tour-counter">
              <span class="counter-current">{{ currentStepIndex + 1 }}</span>
              <span class="counter-sep">/</span>
              <span class="counter-total">{{ walkthroughSteps.length }}</span>
            </div>

            <button class="tour-close-btn" @click="closeWalkthrough" aria-label="Exit Walkthrough">
              <span>Exit Tour</span> &times;
            </button>
          </header>

          <!-- Main Interactive Room Viewport -->
          <div class="tour-viewport">
            <div class="tour-image-stage">
              <Transition name="fade-slide" mode="out-in">
                <img
                  :key="currentStep.id"
                  :src="currentStep.image"
                  :alt="currentStep.name"
                  class="tour-image"
                />
              </Transition>

              <!-- Room Floating Label -->
              <div class="room-pill">
                <span class="room-pill-level">{{ currentStep.level }}</span>
                <span class="room-pill-title">{{ currentStep.name }}</span>
              </div>

              <!-- Quick Next / Prev Arrows on Photo -->
              <button class="stage-nav-arrow prev" @click="prevStep" aria-label="Previous Room">
                &#8592;
              </button>
              <button class="stage-nav-arrow next" @click="nextStep" aria-label="Next Room">
                &#8594;
              </button>
            </div>

            <!-- Bottom Spatial Advancement & Decision Hub -->
            <div class="tour-nav-dock">
              <div class="dock-header">
                <div class="dock-title-block">
                  <span class="dock-label">CURRENT LOCATION</span>
                  <h3 class="dock-room-name">{{ currentStep.name }}</h3>
                  <p class="dock-room-desc">{{ currentStep.description }}</p>
                </div>

                <!-- Sequential advancement buttons -->
                <div class="step-nav-buttons">
                  <button class="step-btn" @click="prevStep">
                    &#8592; Prev Room
                  </button>
                  <button class="step-btn next-step-btn" @click="nextStep">
                    Next Room &#8594;
                  </button>
                </div>
              </div>

              <!-- Spatial Room-to-Room Navigation Buttons -->
              <div class="spatial-actions-wrapper">
                <span class="spatial-prompt">MOVE THROUGH THE HOUSE:</span>
                <div class="spatial-buttons">
                  <button
                    v-for="(action, aIdx) in currentStep.actions"
                    :key="aIdx"
                    class="spatial-btn"
                    @click="navigateToRoom(action.targetId)"
                  >
                    {{ action.label }}
                  </button>
                </div>
              </div>

              <!-- Mini Room Indicator Pills -->
              <div class="room-pills-strip">
                <button
                  v-for="(step, sIdx) in walkthroughSteps"
                  :key="step.id"
                  class="strip-dot"
                  :class="{ 'is-active': sIdx === currentStepIndex }"
                  :title="step.name"
                  @click="currentStepIndex = sIdx"
                >
                  <span class="strip-label">{{ sIdx + 1 }}. {{ step.name }}</span>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </Teleport>

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
  margin-bottom: 3rem;
}

.project-description p {
  font-size: 1.05rem;
  line-height: 1.85;
  color: #55524d;
  font-weight: 300;
  margin: 0;
}

/* --- Walkthrough CTA Banner --- */
.walkthrough-cta-box {
  margin-top: 2rem;
  padding: 1.8rem 2rem;
  background: linear-gradient(135deg, #1f1e1c 0%, #171615 100%);
  border: 1px solid rgba(212, 175, 55, 0.35);
  border-radius: 3px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 2rem;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.12);
}

.cta-badge {
  font-size: 0.68rem;
  letter-spacing: 2.5px;
  color: #d4af37;
  font-weight: 600;
  text-transform: uppercase;
  display: block;
  margin-bottom: 0.4rem;
}

.cta-title {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 1.55rem;
  font-weight: 400;
  color: #ffffff;
  margin: 0 0 0.4rem 0;
}

.cta-desc {
  font-size: 0.88rem;
  line-height: 1.6;
  color: #b0aca5;
  margin: 0;
  font-weight: 300;
  max-width: 500px;
}

.walkthrough-btn {
  background-color: #d4af37;
  color: #171615;
  border: none;
  padding: 0.95rem 1.8rem;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.82rem;
  font-weight: 600;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  cursor: pointer;
  white-space: nowrap;
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(212, 175, 55, 0.3);
}

.walkthrough-btn:hover {
  background-color: #ffffff;
  color: #171615;
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 255, 255, 0.25);
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

/* --- Walkthrough Modal --- */
.walkthrough-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(10, 9, 8, 0.96);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  z-index: 2000;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.walkthrough-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100vw;
  box-sizing: border-box;
}

/* Walkthrough Top Bar */
.tour-top-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.2rem 3rem;
  border-bottom: 1px solid rgba(212, 175, 55, 0.2);
  background-color: rgba(20, 19, 18, 0.6);
  z-index: 10;
}

.tour-branding {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  font-size: 0.85rem;
  letter-spacing: 3px;
  color: #d4af37;
  font-weight: 600;
}

.tour-brand-sep {
  opacity: 0.4;
}

.tour-brand-sub {
  color: #a39f97;
  font-weight: 400;
}

.tour-counter {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 1.4rem;
  color: #d4af37;
  letter-spacing: 2px;
}

.counter-sep {
  color: #6d6a64;
  margin: 0 0.3rem;
}

.counter-total {
  color: #8f8b83;
}

.tour-close-btn {
  background: none;
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: #f2eee8;
  padding: 0.5rem 1.2rem;
  border-radius: 20px;
  font-size: 0.8rem;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.2s ease;
}

.tour-close-btn:hover {
  border-color: #d4af37;
  color: #d4af37;
  background-color: rgba(212, 175, 55, 0.1);
}

/* Main Viewport */
.tour-viewport {
  flex: 1;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  position: relative;
}

.tour-image-stage {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #0b0a09;
  overflow: hidden;
}

.tour-image {
  max-width: 100%;
  max-height: 100%;
  width: 100%;
  height: 100%;
  object-fit: contain;
}

/* Floating Room Pill */
.room-pill {
  position: absolute;
  top: 1.8rem;
  left: 2rem;
  background: rgba(18, 17, 16, 0.8);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(212, 175, 55, 0.35);
  padding: 0.7rem 1.4rem;
  border-radius: 2px;
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
  pointer-events: none;
}

.room-pill-level {
  font-size: 0.65rem;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: #d4af37;
  font-weight: 600;
}

.room-pill-title {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 1.35rem;
  color: #ffffff;
  font-weight: 400;
}

/* Stage Navigation Arrows */
.stage-nav-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(20, 19, 18, 0.6);
  border: 1px solid rgba(255, 255, 255, 0.15);
  color: #ffffff;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  font-size: 1.3rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.25s ease;
  backdrop-filter: blur(6px);
}

.stage-nav-arrow.prev {
  left: 2rem;
}

.stage-nav-arrow.next {
  right: 2rem;
}

.stage-nav-arrow:hover {
  background-color: #d4af37;
  color: #171615;
  border-color: #d4af37;
}

/* Bottom Navigation Dock */
.tour-nav-dock {
  background: linear-gradient(180deg, rgba(22, 21, 20, 0.95) 0%, #11100f 100%);
  border-top: 1px solid rgba(212, 175, 55, 0.25);
  padding: 1.5rem 3rem 1.2rem;
  display: flex;
  flex-direction: column;
  gap: 1.2rem;
  z-index: 10;
}

.dock-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 2rem;
}

.dock-title-block {
  max-width: 650px;
}

.dock-label {
  font-size: 0.65rem;
  letter-spacing: 2px;
  color: #d4af37;
  font-weight: 600;
  text-transform: uppercase;
}

.dock-room-name {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 1.6rem;
  font-weight: 400;
  color: #ffffff;
  margin: 0.2rem 0 0.4rem 0;
}

.dock-room-desc {
  font-size: 0.88rem;
  line-height: 1.6;
  color: #aba69e;
  margin: 0;
  font-weight: 300;
}

.step-nav-buttons {
  display: flex;
  gap: 0.8rem;
  white-space: nowrap;
}

.step-btn {
  background: none;
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: #e5e2dc;
  padding: 0.7rem 1.3rem;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.8rem;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.25s ease;
}

.step-btn:hover {
  border-color: #d4af37;
  color: #d4af37;
}

.next-step-btn {
  background-color: #d4af37;
  border-color: #d4af37;
  color: #171615;
  font-weight: 600;
}

.next-step-btn:hover {
  background-color: #ffffff;
  border-color: #ffffff;
  color: #171615;
}

/* Spatial Room Choices (Bedroom to back etc.) */
.spatial-actions-wrapper {
  display: flex;
  align-items: center;
  gap: 1.2rem;
  padding-top: 0.6rem;
  border-top: 1px solid rgba(255, 255, 255, 0.08);
  flex-wrap: wrap;
}

.spatial-prompt {
  font-size: 0.7rem;
  letter-spacing: 2px;
  color: #d4af37;
  font-weight: 600;
}

.spatial-buttons {
  display: flex;
  gap: 0.8rem;
  flex-wrap: wrap;
}

.spatial-btn {
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(212, 175, 55, 0.4);
  color: #f7f4ee;
  padding: 0.55rem 1.1rem;
  border-radius: 2px;
  font-size: 0.8rem;
  letter-spacing: 0.8px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.spatial-btn:hover {
  background-color: #d4af37;
  color: #171615;
  border-color: #d4af37;
  transform: translateY(-1px);
}

/* Room Indicator Pills Strip */
.room-pills-strip {
  display: flex;
  gap: 0.5rem;
  overflow-x: auto;
  padding-bottom: 0.4rem;
  scrollbar-width: thin;
}

.strip-dot {
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: #8c8983;
  padding: 0.35rem 0.8rem;
  border-radius: 14px;
  font-size: 0.7rem;
  cursor: pointer;
  white-space: nowrap;
  transition: all 0.2s ease;
}

.strip-dot:hover {
  color: #ffffff;
  border-color: rgba(212, 175, 55, 0.4);
}

.strip-dot.is-active {
  background-color: #d4af37;
  color: #171615;
  border-color: #d4af37;
  font-weight: 600;
}

/* Transition Animations */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: opacity 0.35s ease, transform 0.35s ease;
}

.fade-slide-enter-from {
  opacity: 0;
  transform: scale(0.98);
}

.fade-slide-leave-to {
  opacity: 0;
  transform: scale(1.02);
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
  .walkthrough-cta-box {
    flex-direction: column;
    align-items: flex-start;
  }
  .dock-header {
    flex-direction: column;
    gap: 1rem;
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

  /* Tour Modal adjustments */
  .tour-top-bar {
    padding: 0.9rem 1.25rem;
  }

  .tour-brand-sub,
  .tour-brand-sep {
    display: none;
  }

  .tour-nav-dock {
    padding: 1.2rem 1.25rem;
  }

  .stage-nav-arrow {
    display: none;
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

  .walkthrough-cta-box {
    padding: 1.25rem 1rem;
    gap: 1rem;
  }

  .cta-title {
    font-size: 1.3rem;
  }

  .cta-desc {
    font-size: 0.82rem;
  }

  .walkthrough-btn {
    width: 100%;
    justify-content: center;
    padding: 0.85rem 1.2rem;
    font-size: 0.78rem;
  }

  .size-tall .image-inner img,
  .size-portrait .image-inner img,
  .size-medium .image-inner img,
  .size-wide .image-inner img,
  .size-square .image-inner img {
    height: 260px;
  }

  /* Tour Modal Mobile Fit */
  .tour-top-bar {
    padding: 0.75rem 1rem;
  }

  .tour-image-stage {
    max-height: 42vh;
  }

  .room-pill {
    top: 0.6rem;
    left: 0.6rem;
    padding: 0.35rem 0.7rem;
  }

  .room-pill-title {
    font-size: 1.05rem;
  }

  .tour-nav-dock {
    padding: 1rem 1rem 1.2rem;
    gap: 0.9rem;
    overflow-y: auto;
  }

  .dock-room-name {
    font-size: 1.35rem;
  }

  .dock-room-desc {
    font-size: 0.8rem;
    line-height: 1.5;
  }

  .step-nav-buttons {
    width: 100%;
  }

  .step-btn {
    flex: 1;
    text-align: center;
    padding: 0.65rem 0.5rem;
    font-size: 0.75rem;
  }

  .spatial-buttons {
    width: 100%;
  }

  .spatial-btn {
    width: 100%;
    font-size: 0.76rem;
    padding: 0.55rem 0.8rem;
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
