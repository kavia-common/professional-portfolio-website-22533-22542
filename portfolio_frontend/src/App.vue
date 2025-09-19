<script setup lang="ts">
import { onMounted, ref } from 'vue'
import SkillsSection from './components/SkillsSection.vue'

const isMenuOpen = ref(false)
const activeSection = ref('home')

// PUBLIC_INTERFACE
function scrollToId(id: string) {
  /** Smoothly scroll to a section and update active link */
  const el = document.getElementById(id)
  if (el) el.scrollIntoView({ behavior: 'smooth', block: 'start' })
  isMenuOpen.value = false
}

onMounted(() => {
  // Observe sections to update active nav state
  const sections = document.querySelectorAll('section[data-section]')
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const id = (entry.target as HTMLElement).id
          activeSection.value = id
        }
      })
    },
    { rootMargin: '-40% 0px -55% 0px', threshold: [0, 0.25, 0.5, 0.75, 1] },
  )
  sections.forEach((s) => observer.observe(s))
})
</script>

<template>
  <div class="app-shell">
    <!-- Sticky Navigation -->
    <header class="nav">
      <div class="nav-inner">
        <a class="brand" href="#" @click.prevent="scrollToId('home')">
          <span class="brand-mark">OP</span>
          <span class="brand-text">Ocean Professional</span>
        </a>

        <nav class="nav-links" :class="{ open: isMenuOpen }" aria-label="Primary Navigation">
          <a :class="{ active: activeSection === 'home' }" href="#home" @click.prevent="scrollToId('home')">Home</a>
          <a :class="{ active: activeSection === 'about' }" href="#about" @click.prevent="scrollToId('about')">About</a>
          <a :class="{ active: activeSection === 'skills' }" href="#skills" @click.prevent="scrollToId('skills')">Skills</a>
          <a :class="{ active: activeSection === 'projects' }" href="#projects" @click.prevent="scrollToId('projects')">Projects</a>
          <a :class="{ active: activeSection === 'contact' }" href="#contact" @click.prevent="scrollToId('contact')">Contact</a>
        </nav>

        <button class="menu-btn" aria-label="Toggle menu" @click="isMenuOpen = !isMenuOpen">
          <span></span><span></span><span></span>
        </button>
      </div>
    </header>

    <main>
      <!-- Hero -->
      <section id="home" data-section class="hero surface-card">
        <div class="hero-content">
          <h1>Hi, I’m <span class="accent">Your Name</span></h1>
          <p class="subtitle">
            Frontend Developer crafting modern, accessible web experiences. I build fast,
            responsive interfaces with a keen eye for detail.
          </p>
          <div class="cta">
            <a class="btn primary" href="#projects" @click.prevent="scrollToId('projects')">View Projects</a>
            <a class="btn secondary" href="#contact" @click.prevent="scrollToId('contact')">Contact Me</a>
          </div>
        </div>
        <div class="hero-visual" aria-hidden="true">
          <div class="orb orb-1"></div>
          <div class="orb orb-2"></div>
          <div class="orb orb-3"></div>
        </div>
      </section>

      <!-- About -->
      <section id="about" data-section class="section surface-card">
        <header class="section-header">
          <h2>About</h2>
          <p>Clean code, thoughtful UX, and reliable delivery.</p>
        </header>
        <div class="about-grid">
          <div class="about-card">
            <h3>Who I am</h3>
            <p>
              I’m a developer focused on building performant, maintainable interfaces.
              I enjoy design systems, interaction details, and component architecture.
            </p>
          </div>
          <div class="about-card">
            <h3>What I do</h3>
            <ul>
              <li>Design system-driven UI development</li>
              <li>Vue 3, TypeScript, Vite and modern tooling</li>
              <li>Responsive, accessible, and SEO-friendly</li>
            </ul>
          </div>
        </div>
      </section>

      <!-- Skills (modern component) -->
      <SkillsSection class="section" />

      <!-- Projects -->
      <section id="projects" data-section class="section surface-card">
        <header class="section-header">
          <h2>Projects</h2>
          <p>Selected work with focus on UX, performance, and clarity.</p>
        </header>
        <div class="cards">
          <article class="card">
            <div class="card-media"></div>
            <div class="card-body">
              <h3>Dashboard Analytics</h3>
              <p>A modular analytics dashboard with custom charts, theme support, and role-based access.</p>
              <div class="card-actions">
                <a class="btn link" href="#" aria-label="Open live demo">Live</a>
                <a class="btn link" href="#" aria-label="Open source code">Code</a>
              </div>
            </div>
          </article>

          <article class="card">
            <div class="card-media"></div>
            <div class="card-body">
              <h3>Design System Kit</h3>
              <p>A Vue-based component library with tokens, docs, and theming utilities.</p>
              <div class="card-actions">
                <a class="btn link" href="#" aria-label="Open live demo">Live</a>
                <a class="btn link" href="#" aria-label="Open source code">Code</a>
              </div>
            </div>
          </article>

          <article class="card">
            <div class="card-media"></div>
            <div class="card-body">
              <h3>Marketing Site</h3>
              <p>High-performing static site with smooth animations and a11y-focused navigation.</p>
              <div class="card-actions">
                <a class="btn link" href="#" aria-label="Open live demo">Live</a>
                <a class="btn link" href="#" aria-label="Open source code">Code</a>
              </div>
            </div>
          </article>
        </div>
      </section>

      <!-- Contact -->
      <section id="contact" data-section class="section surface-card">
        <header class="section-header">
          <h2>Contact</h2>
          <p>Let’s build something great together.</p>
        </header>
        <form class="contact-form" @submit.prevent>
          <div class="grid">
            <label class="field">
              <span>Name</span>
              <input type="text" name="name" placeholder="Your name" required />
            </label>
            <label class="field">
              <span>Email</span>
              <input type="email" name="email" placeholder="you@example.com" required />
            </label>
          </div>
          <label class="field">
            <span>Message</span>
            <textarea name="message" rows="5" placeholder="Tell me about your project..." required></textarea>
          </label>
          <button class="btn primary" type="submit">Send Message</button>
          <p class="form-note">This is a demo form. Hook it up to your backend or service.</p>
        </form>
      </section>
    </main>

    <footer class="footer">
      <p>© {{ new Date().getFullYear() }} Your Name · Built with Vue 3</p>
    </footer>
  </div>
</template>

<style scoped>
/* Color tokens - Ocean Professional */
:root {
  --primary: #2563EB;
  --secondary: #F59E0B;
  --success: #F59E0B;
  --error: #EF4444;
  --background: #f9fafb;
  --surface: #ffffff;
  --text: #111827;
  --gradientA: rgba(59, 130, 246, 0.10); /* blue-500/10 */
  --gradientB: #f9fafb; /* gray-50 */
}

.app-shell {
  background: var(--background);
  color: var(--text);
}

/* Sticky Nav */
.nav {
  position: sticky;
  top: 0;
  z-index: 50;
  background: linear-gradient(180deg, var(--surface), rgba(255,255,255,0.96));
  border-bottom: 1px solid rgba(17, 24, 39, 0.06);
  backdrop-filter: saturate(180%) blur(8px);
}
.nav-inner {
  max-width: 1100px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  padding: 0.75rem 1rem;
  gap: 1rem;
}
.brand {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  color: var(--text);
}
.brand:hover .brand-text { color: var(--primary); }
.brand-mark {
  background: radial-gradient(100% 100% at 30% 20%, var(--primary) 0%, rgba(37, 99, 235, 0.85) 60%, rgba(37, 99, 235, 0.6) 100%);
  color: white;
  width: 34px; height: 34px;
  display: grid; place-items: center;
  border-radius: 10px;
  box-shadow: 0 6px 18px rgba(37, 99, 235, 0.35);
  font-weight: 700;
  letter-spacing: 0.5px;
}
.brand-text { font-weight: 600; transition: color .2s ease; }

/* Nav links */
.nav-links {
  margin-left: auto;
  display: flex;
  gap: 1rem;
}
.nav-links a {
  position: relative;
  padding: 0.5rem 0.75rem;
  border-radius: 8px;
  color: var(--text);
  transition: color .2s ease, background-color .2s ease, box-shadow .2s ease;
}
.nav-links a:hover {
  color: var(--primary);
  background: rgba(37,99,235,0.08);
}
.nav-links a.active {
  color: var(--primary);
  box-shadow: inset 0 0 0 1px rgba(37,99,235,0.35);
  background: linear-gradient(180deg, rgba(37,99,235,0.08), rgba(255,255,255,0.2));
}

/* Mobile menu */
.menu-btn {
  display: none;
  flex-direction: column;
  gap: 4px;
  margin-left: auto;
  border: none;
  background: transparent;
  padding: 6px;
}
.menu-btn span {
  width: 22px; height: 2px; background: var(--text); display: block;
}
@media (max-width: 820px) {
  .menu-btn { display: inline-flex; }
  .nav-links {
    position: absolute;
    top: 56px; right: 12px; left: 12px;
    background: var(--surface);
    border: 1px solid rgba(17,24,39,0.1);
    border-radius: 12px;
    padding: 0.5rem;
    display: none;
    flex-direction: column;
    box-shadow: 0 10px 30px rgba(17,24,39,0.12);
  }
  .nav-links.open { display: flex; }
}

/* Layout sections */
main { max-width: 1100px; margin: 0 auto; padding: 1rem; }
.section { margin-top: 2rem; border-radius: 16px; padding: 2rem; transition: box-shadow .2s ease, transform .2s ease; }
.section:hover { transform: translateY(-1px); box-shadow: 0 12px 30px rgba(17,24,39,0.08); }
.surface-card {
  position: relative;
  background: var(--surface);
  border: 1px solid rgba(17,24,39,0.06);
  box-shadow: 0 10px 30px rgba(17,24,39,0.06);
  overflow: hidden;
}
.section-header { margin-bottom: 1rem; }
.section-header h2 {
  font-size: 1.75rem; line-height: 1.2; margin-bottom: 0.25rem;
}
.section-header p { color: rgba(17,24,39,0.75); }

/* Hero */
.hero {
  margin-top: 1rem;
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  gap: 1rem;
  padding: 2.5rem;
  background: linear-gradient(135deg, var(--gradientA), var(--gradientB));
}
.hero-content h1 {
  font-size: 2.25rem;
  line-height: 1.15;
  letter-spacing: -0.01em;
}
.accent { color: var(--primary); }
.subtitle { margin-top: 0.75rem; color: rgba(17,24,39,0.75); }
.cta { margin-top: 1.25rem; display: flex; gap: 0.75rem; flex-wrap: wrap; }
.hero-visual { position: relative; min-height: 220px; }
.orb {
  position: absolute; border-radius: 50%;
  filter: blur(16px);
  opacity: 0.9;
}
.orb-1 { width: 180px; height: 180px; top: 10%; right: 10%; background: rgba(37,99,235,0.35); }
.orb-2 { width: 120px; height: 120px; bottom: 5%; right: 25%; background: rgba(245,158,11,0.45); }
.orb-3 { width: 80px; height: 80px; top: 50%; right: 0%; background: rgba(37,99,235,0.25); }

/* About grid */
.about-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}
.about-card {
  background: var(--surface);
  border: 1px solid rgba(17,24,39,0.06);
  border-radius: 14px;
  padding: 1rem;
  box-shadow: 0 8px 24px rgba(17,24,39,0.05);
}
.about-card h3 { margin-bottom: 0.5rem; }
.about-card ul { padding-left: 1.1rem; }
.about-card li { margin: 0.25rem 0; }

/* Skills chips */
.chips { display: flex; flex-wrap: wrap; gap: 0.5rem; }
.chip {
  border-radius: 999px;
  padding: 0.4rem 0.75rem;
  border: 1px solid rgba(17,24,39,0.08);
  background: linear-gradient(180deg, rgba(37,99,235,0.06), rgba(255,255,255,0.7));
  color: var(--text);
}

/* Project Cards */
.cards {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}
.card {
  border-radius: 16px;
  overflow: hidden;
  border: 1px solid rgba(17,24,39,0.06);
  background: var(--surface);
  transition: transform .18s ease, box-shadow .18s ease, border-color .18s ease;
  box-shadow: 0 10px 24px rgba(17,24,39,0.06);
}
.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 16px 36px rgba(17,24,39,0.10);
  border-color: rgba(37,99,235,0.35);
}
.card-media {
  height: 140px;
  background: linear-gradient(135deg, rgba(37,99,235,0.18), rgba(245,158,11,0.18));
}
.card-body { padding: 1rem; }
.card-body h3 { margin-bottom: 0.35rem; }
.card-actions { margin-top: 0.5rem; display: flex; gap: 0.5rem; }

/* Contact form */
.contact-form {
  display: block;
  background: var(--surface);
  border: 1px solid rgba(17,24,39,0.06);
  border-radius: 14px;
  padding: 1rem;
  box-shadow: inset 0 1px 0 rgba(255,255,255,0.35), 0 8px 24px rgba(17,24,39,0.05);
}
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.75rem;
}
.field { display: flex; flex-direction: column; gap: 0.4rem; }
.field span { font-size: 0.9rem; color: rgba(17,24,39,0.85); }
.field input, .field textarea {
  border: 1px solid rgba(17,24,39,0.12);
  border-radius: 10px;
  padding: 0.6rem 0.75rem;
  background: #fff;
  color: var(--text);
  outline: none;
  transition: border-color .15s ease, box-shadow .15s ease;
}
.field input:focus, .field textarea:focus {
  border-color: rgba(37,99,235,0.6);
  box-shadow: 0 0 0 3px rgba(37,99,235,0.15);
}
.contact-form .btn { margin-top: 0.75rem; }
.form-note { margin-top: 0.5rem; font-size: 0.85rem; color: rgba(17,24,39,0.7); }

/* Buttons */
.btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  border-radius: 10px;
  padding: 0.6rem 0.9rem;
  font-weight: 600;
  text-decoration: none;
  transition: background-color .18s ease, box-shadow .18s ease, color .18s ease, transform .05s ease;
  cursor: pointer;
}
.btn:active { transform: translateY(1px); }
.btn.primary {
  background: var(--primary);
  color: #fff;
  box-shadow: 0 10px 24px rgba(37,99,235,0.25);
}
.btn.primary:hover { background: #1d4fd8; }
.btn.secondary {
  background: #fff;
  color: var(--primary);
  border: 1px solid rgba(37,99,235,0.35);
}
.btn.secondary:hover { background: rgba(37,99,235,0.06); }
.btn.link {
  color: var(--primary);
  padding: 0.4rem 0.5rem;
  border-radius: 8px;
}
.btn.link:hover { background: rgba(37,99,235,0.08); }

/* Footer */
.footer {
  max-width: 1100px;
  margin: 2rem auto 3rem;
  padding: 0 1rem;
  color: rgba(17,24,39,0.7);
}

/* Responsive */
@media (max-width: 1024px) {
  .hero { grid-template-columns: 1fr; }
  .about-grid { grid-template-columns: 1fr; }
  .cards { grid-template-columns: 1fr 1fr; }
}
@media (max-width: 640px) {
  main { padding: 0.75rem; }
  .section { padding: 1.25rem; }
  .cards { grid-template-columns: 1fr; }
  .grid { grid-template-columns: 1fr; }
}
</style>
