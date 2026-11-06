<script setup>
import { reactive, ref } from 'vue'

const form = reactive({
  name: '',
  company: '',
  address: '',
  phone: '',
  email: '',
  description: '',
})

const isSubmitting = ref(false)
const isSubmitted = ref(false)
const errorMessage = ref('')

const handleSubmit = async () => {
  errorMessage.value = ''
  
  if (!form.name || !form.email || !form.phone || !form.description) {
    errorMessage.value = 'Please complete all required fields (Name, Phone, Email, and Description).'
    return
  }

  isSubmitting.value = true

  // Simulate network request for form submission
  setTimeout(() => {
    isSubmitting.value = false
    isSubmitted.value = true
  }, 800)
}

const resetForm = () => {
  form.name = ''
  form.company = ''
  form.address = ''
  form.phone = ''
  form.email = ''
  form.description = ''
  isSubmitted.value = false
  errorMessage.value = ''
}
</script>

<template>
  <div class="contact-page">
    <!-- Header -->
    <header class="page-header">
      <span class="tag">GET IN TOUCH · PORTO RAFTI</span>
      <h1 class="page-title">Contact Moto-X</h1>
      <div class="divider"></div>
      <p class="header-desc">
        We welcome discussions regarding bespoke residences, coastal land acquisitions, and custom villa
        developments across Porto Rafti and the Attica coastline.
      </p>
    </header>

    <!-- Main Content Grid -->
    <main class="contact-wrapper">
      <div class="contact-grid">
        <!-- Left: Direct Office & Contact Details -->
        <aside class="info-card">
          <div class="info-header">
            <span class="info-tag">HEADQUARTERS</span>
            <h2 class="info-title">Studio & Office</h2>
            <p class="info-lead">
              Our development studio is situated in Porto Rafti. Private site visits and project consultations are
              available by appointment.
            </p>
          </div>

          <div class="info-items">
            <!-- Address -->
            <div class="info-item">
              <div class="icon-circle">📍</div>
              <div class="item-text">
                <span class="item-label">OFFICE ADDRESS</span>
                <p class="item-value">Krionerio 11</p>
                <p class="item-sub">Porto Rafti 19003, Attica, Greece</p>
                <a
                  href="https://maps.google.com/?q=Krionerio+11+Porto+Rafti+19003"
                  target="_blank"
                  rel="noopener"
                  class="item-link"
                >
                  View on Google Maps &rarr;
                </a>
              </div>
            </div>

            <!-- Phone -->
            <div class="info-item">
              <div class="icon-circle">📞</div>
              <div class="item-text">
                <span class="item-label">DIRECT TELEPHONE / CELL</span>
                <a href="tel:+306986896903" class="phone-link">+30 698 6896903</a>
                <p class="item-sub">Available Mon – Sat, 09:00 – 19:00 EET</p>
              </div>
            </div>

            <!-- Email -->
            <div class="info-item">
              <div class="icon-circle">✉️</div>
              <div class="item-text">
                <span class="item-label">PRIMARY INQUIRIES</span>
                <a href="mailto:info@motoxpd.gr" class="email-link">info@motoxpd.gr</a>
                <p class="item-sub">
                  Direct inquiries are forwarded immediately to our senior executive team.
                </p>
              </div>
            </div>
          </div>

          <!-- Highlight Box -->
          <div class="location-banner">
            <span class="banner-title">BESPOKE DEVELOPMENT</span>
            <p class="banner-desc">
              Have a plot or looking to build a private estate in Porto Rafti? Our architects and project managers
              provide turnkey solutions from initial permits to handover.
            </p>
          </div>
        </aside>

        <!-- Right: Prospect & Client Form -->
        <section class="form-card">
          <div v-if="!isSubmitted" class="form-container">
            <div class="form-header">
              <span class="info-tag">INQUIRY FORM</span>
              <h2 class="form-title">Start a Conversation</h2>
              <p class="form-intro">
                Please complete the details below and describe what you are looking for. Our team will review your
                requirements and reply promptly.
              </p>
            </div>

            <form @submit.prevent="handleSubmit" class="inquiry-form" novalidate>
              <!-- Name & Company (Row) -->
              <div class="form-row">
                <div class="form-group">
                  <label for="name">
                    Full Name <span class="required">*</span>
                  </label>
                  <input
                    id="name"
                    v-model="form.name"
                    type="text"
                    placeholder="e.g. Alexander Drake"
                    required
                  />
                </div>

                <div class="form-group">
                  <label for="company">
                    Company <span class="optional">(Optional)</span>
                  </label>
                  <input
                    id="company"
                    v-model="form.company"
                    type="text"
                    placeholder="e.g. Drake Capital / Private"
                  />
                </div>
              </div>

              <!-- Phone & Email (Row) -->
              <div class="form-row">
                <div class="form-group">
                  <label for="phone">
                    Cell or Phone # <span class="required">*</span>
                  </label>
                  <input
                    id="phone"
                    v-model="form.phone"
                    type="tel"
                    placeholder="+30 690 0000000"
                    required
                  />
                </div>

                <div class="form-group">
                  <label for="email">
                    Email Address <span class="required">*</span>
                  </label>
                  <input
                    id="email"
                    v-model="form.email"
                    type="email"
                    placeholder="alexander@example.com"
                    required
                  />
                </div>
              </div>

              <!-- Address -->
              <div class="form-group">
                <label for="address">
                  Client / Property Address <span class="optional">(Optional)</span>
                </label>
                <input
                  id="address"
                  v-model="form.address"
                  type="text"
                  placeholder="e.g. City, Country or Target Location in Attica"
                />
              </div>

              <!-- Description Area -->
              <div class="form-group">
                <label for="description">
                  Project Description & Requirements <span class="required">*</span>
                </label>
                <textarea
                  id="description"
                  v-model="form.description"
                  rows="5"
                  placeholder="Explain what you are looking for (e.g. acquiring an existing residence like Raptis, custom villa construction, timeline, desired square meters, sea view requirements)..."
                  required
                ></textarea>
              </div>

              <!-- Error Alert -->
              <div v-if="errorMessage" class="error-banner">
                {{ errorMessage }}
              </div>

              <!-- Submit Button -->
              <button
                type="submit"
                class="submit-btn"
                :disabled="isSubmitting"
              >
                <span v-if="isSubmitting">Sending Inquiry...</span>
                <span v-else>Send Inquiry &rarr;</span>
              </button>
            </form>
          </div>

          <!-- Success State -->
          <div v-else class="success-container">
            <div class="success-icon">✦</div>
            <span class="info-tag">THANK YOU</span>
            <h2 class="success-title">Inquiry Received</h2>
            <div class="divider"></div>
            <p class="success-desc">
              Thank you, <strong>{{ form.name }}</strong>. Your inquiry has been logged and forwarded directly to our
              executive team (<strong>info@motoxpd.gr</strong>).
            </p>
            <p class="success-sub">
              A project director will review your specifications and contact you via phone ({{ form.phone }}) or
              email ({{ form.email }}).
            </p>

            <button class="reset-btn" @click="resetForm">
              Send Another Inquiry
            </button>
          </div>
        </section>
      </div>
    </main>

    <!-- Minimalist Footer -->
    <footer class="footer">
      <p class="footer-brand">MOTO-X DEVELOPMENT</p>
      <p class="footer-location">Krionerio 11, Porto Rafti 19003, Greece</p>
      <p class="footer-copy">&copy; 2026 Moto-X. All rights reserved.</p>
    </footer>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,400&family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

.contact-page {
  min-height: 100vh;
  background-color: #faf8f5;
  color: #1a1918;
  font-family: 'Montserrat', sans-serif;
  padding-top: 100px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

/* --- Page Header --- */
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

/* --- Contact Wrapper & Grid --- */
.contact-wrapper {
  max-width: 1300px;
  width: 100%;
  margin: 0 auto 6rem;
  padding: 0 2rem;
  box-sizing: border-box;
  flex: 1;
}

.contact-grid {
  display: grid;
  grid-template-columns: 1fr 1.35fr;
  gap: 3.5rem;
  align-items: stretch;
}

/* --- Left: Info Card --- */
.info-card {
  background-color: #ffffff;
  border: 1px solid rgba(0, 0, 0, 0.06);
  border-radius: 2px;
  padding: 3.5rem 3rem;
  display: flex;
  flex-direction: column;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.02);
}

.info-tag {
  font-size: 0.7rem;
  letter-spacing: 2.5px;
  color: #d4af37;
  font-weight: 600;
  text-transform: uppercase;
  display: block;
  margin-bottom: 0.5rem;
}

.info-title {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 2.2rem;
  font-weight: 400;
  color: #171615;
  margin: 0 0 1rem 0;
  letter-spacing: -0.3px;
}

.info-lead {
  font-size: 0.95rem;
  line-height: 1.75;
  color: #63605a;
  font-weight: 300;
  margin: 0 0 2.5rem 0;
}

.info-items {
  display: flex;
  flex-direction: column;
  gap: 2.2rem;
  margin-bottom: 3rem;
}

.info-item {
  display: flex;
  align-items: flex-start;
  gap: 1.2rem;
}

.icon-circle {
  width: 42px;
  height: 42px;
  border-radius: 50%;
  background-color: #f7f5f2;
  border: 1px solid rgba(212, 175, 55, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.1rem;
  flex-shrink: 0;
}

.item-text {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.item-label {
  font-size: 0.68rem;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: #9c9891;
  font-weight: 600;
}

.item-value {
  font-size: 1.1rem;
  font-weight: 500;
  color: #171615;
  margin: 0;
}

.phone-link,
.email-link {
  font-size: 1.1rem;
  font-weight: 500;
  color: #171615;
  text-decoration: none;
  transition: color 0.2s ease;
}

.phone-link:hover,
.email-link:hover {
  color: #d4af37;
}

.item-sub {
  font-size: 0.85rem;
  color: #7a7670;
  line-height: 1.5;
  margin: 0;
  font-weight: 300;
}

.item-link {
  display: inline-block;
  margin-top: 0.3rem;
  font-size: 0.82rem;
  color: #d4af37;
  text-decoration: none;
  font-weight: 600;
  letter-spacing: 1px;
  transition: transform 0.2s ease;
}

.item-link:hover {
  transform: translateX(4px);
}

.location-banner {
  margin-top: auto;
  padding: 1.8rem;
  background: linear-gradient(135deg, #1f1e1c 0%, #151413 100%);
  border: 1px solid rgba(212, 175, 55, 0.35);
  border-radius: 2px;
  color: #ffffff;
}

.banner-title {
  font-size: 0.7rem;
  letter-spacing: 2.5px;
  color: #d4af37;
  font-weight: 600;
  display: block;
  margin-bottom: 0.4rem;
}

.banner-desc {
  font-size: 0.85rem;
  line-height: 1.65;
  color: #b0aca5;
  margin: 0;
  font-weight: 300;
}

/* --- Right: Form Card --- */
.form-card {
  background-color: #ffffff;
  border: 1px solid rgba(0, 0, 0, 0.06);
  border-radius: 2px;
  padding: 3.5rem 3.5rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.02);
}

.form-header {
  margin-bottom: 2.5rem;
}

.form-title {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 2.2rem;
  font-weight: 400;
  color: #171615;
  margin: 0 0 0.8rem 0;
  letter-spacing: -0.3px;
}

.form-intro {
  font-size: 0.95rem;
  line-height: 1.75;
  color: #63605a;
  font-weight: 300;
  margin: 0;
}

.inquiry-form {
  display: flex;
  flex-direction: column;
  gap: 1.6rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-group label {
  font-size: 0.78rem;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  color: #2b2927;
  font-weight: 600;
}

.required {
  color: #d4af37;
}

.optional {
  color: #9e9a93;
  font-size: 0.7rem;
  text-transform: none;
  font-weight: 400;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 0.95rem 1.1rem;
  background-color: #faf8f5;
  border: 1px solid rgba(0, 0, 0, 0.1);
  border-radius: 2px;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.95rem;
  color: #171615;
  box-sizing: border-box;
  transition: border-color 0.25s ease, background-color 0.25s ease, box-shadow 0.25s ease;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  background-color: #ffffff;
  border-color: #d4af37;
  box-shadow: 0 0 0 3px rgba(212, 175, 55, 0.15);
}

.form-group textarea {
  resize: vertical;
  min-height: 120px;
  line-height: 1.6;
}

.error-banner {
  background-color: #fdf2f2;
  border-left: 3px solid #d9534f;
  color: #b52b27;
  padding: 0.8rem 1rem;
  font-size: 0.85rem;
  border-radius: 2px;
}

.submit-btn {
  background-color: #171615;
  color: #d4af37;
  border: 1px solid #d4af37;
  padding: 1.1rem 2rem;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 0.5rem;
}

.submit-btn:hover:not(:disabled) {
  background-color: #d4af37;
  color: #171615;
  box-shadow: 0 8px 25px rgba(212, 175, 55, 0.25);
  transform: translateY(-2px);
}

.submit-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

/* --- Success State --- */
.success-container {
  text-align: center;
  padding: 3rem 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.success-icon {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  border: 1px solid #d4af37;
  color: #d4af37;
  font-size: 1.8rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 1.5rem;
  background-color: #faf8f5;
}

.success-title {
  font-family: 'Cormorant Garamond', Georgia, serif;
  font-size: 2.6rem;
  font-weight: 300;
  color: #151413;
  margin: 0;
}

.success-desc {
  font-size: 1.05rem;
  line-height: 1.8;
  color: #3d3b37;
  max-width: 520px;
  margin: 0 auto 0.8rem;
  font-weight: 300;
}

.success-sub {
  font-size: 0.9rem;
  line-height: 1.7;
  color: #7d7973;
  max-width: 500px;
  margin: 0 auto 2.5rem;
  font-weight: 300;
}

.reset-btn {
  background: none;
  border: 1px solid #d4af37;
  color: #171615;
  padding: 0.85rem 2rem;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.25s ease;
}

.reset-btn:hover {
  background-color: #d4af37;
  color: #171615;
}

/* --- Footer --- */
.footer {
  background-color: #141312;
  color: #8c8983;
  text-align: center;
  padding: 3.5rem 2rem;
  border-top: 1px solid rgba(212, 175, 55, 0.15);
  margin-top: auto;
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

/* --- Responsive Adjustments --- */
@media (max-width: 992px) {
  .contact-grid {
    grid-template-columns: 1fr;
    gap: 2.5rem;
  }
  .info-card,
  .form-card {
    padding: 2.5rem 2rem;
  }
}

@media (max-width: 600px) {
  .contact-page {
    padding-top: 80px;
  }
  .form-row {
    grid-template-columns: 1fr;
    gap: 1.2rem;
  }
  .page-header {
    padding: 3rem 1.5rem 2rem;
  }
  .contact-wrapper {
    padding: 0 1.2rem;
  }
}
</style>

