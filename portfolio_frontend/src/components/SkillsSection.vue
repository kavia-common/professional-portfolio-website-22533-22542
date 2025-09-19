<script setup lang="ts">
import { ref, onMounted } from 'vue'

type Skill = {
  name: string
  level: number // 0-100
  icon: string  // emoji or icon text
}

const coreSkills = ref<Skill[]>([
  { name: 'Vue 3', level: 92, icon: '🟢' },
  { name: 'TypeScript', level: 88, icon: '🔷' },
  { name: 'Pinia', level: 84, icon: '🍍' },
  { name: 'Vite', level: 86, icon: '⚡' },
  { name: 'Accessibility', level: 80, icon: '♿' },
  { name: 'Component Design', level: 90, icon: '🧩' },
])

const toolset = ref<string[]>([
  'HTML5', 'CSS3', 'Tailwind', 'Jest/Vitest', 'Playwright', 'Git', 'REST', 'Figma',
])

const animatedLevels = ref<number[]>(coreSkills.value.map(() => 0))

onMounted(() => {
  // Animate radial progress on mount
  requestAnimationFrame(() => {
    coreSkills.value.forEach((s, i) => {
      const target = s.level
      let current = 0
      const step = () => {
        current += Math.max(1, Math.round((target - current) * 0.12))
        if (current >= target) {
          current = target
        } else {
          requestAnimationFrame(step)
        }
        animatedLevels.value[i] = current
      }
      step()
    })
  })
})

// PUBLIC_INTERFACE
function formatLevel(n: number): string {
  /** Format percentage level value for display */
  return `${Math.round(n)}%`
}
</script>

<template>
  <section id="skills" data-section class="skills surface-card">
    <header class="section-header">
      <h2>Skills</h2>
      <p>Core technologies and tools I use daily.</p>
    </header>

    <!-- Core skills with animated radial progress -->
    <div class="skill-grid">
      <article
        v-for="(skill, idx) in coreSkills"
        :key="skill.name"
        class="skill-card"
        :aria-label="`${skill.name} proficiency ${formatLevel(skill.level)}`"
      >
        <div class="radial-wrap">
          <svg class="radial" viewBox="0 0 120 120" aria-hidden="true">
            <!-- track -->
            <circle class="track" cx="60" cy="60" r="46" />
            <!-- progress -->
            <circle
              class="progress"
              cx="60" cy="60" r="46"
              :style="{
                strokeDasharray: 2 * Math.PI * 46 + 'px',
                strokeDashoffset: (2 * Math.PI * 46) * (1 - animatedLevels[idx] / 100) + 'px'
              }"
            />
          </svg>
          <span class="icon" :title="skill.name">{{ skill.icon }}</span>
          <span class="percent">{{ formatLevel(animatedLevels[idx]) }}</span>
        </div>
        <h3>{{ skill.name }}</h3>
      </article>
    </div>

    <!-- Tools as interactive chips -->
    <div class="tools">
      <h4>Toolbox</h4>
      <div class="chips">
        <span v-for="tool in toolset" :key="tool" class="chip" role="listitem">{{ tool }}</span>
      </div>
    </div>
  </section>
</template>

<style scoped>
/* Section container, themed background gradient for subtle depth */
.skills {
  border-radius: 16px;
  padding: 1.5rem;
  background: linear-gradient(135deg, var(--gradientA), var(--gradientB));
}

/* Grid of core skills */
.skill-grid {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 0.75rem;
  margin-top: 1rem;
}
@media (max-width: 1100px) {
  .skill-grid { grid-template-columns: repeat(3, 1fr); }
}
@media (max-width: 640px) {
  .skill-grid { grid-template-columns: repeat(2, 1fr); }
}

/* Individual skill card */
.skill-card {
  position: relative;
  background: var(--surface);
  border: 1px solid rgba(17,24,39,0.06);
  border-radius: 14px;
  padding: 0.9rem 0.75rem 0.95rem;
  text-align: center;
  box-shadow: 0 8px 24px rgba(17,24,39,0.06);
  transition: transform .18s ease, box-shadow .18s ease, border-color .18s ease;
}
.skill-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 14px 30px rgba(17,24,39,0.10);
  border-color: rgba(37,99,235,0.25);
}

/* Radial progress */
.radial-wrap {
  position: relative;
  width: 120px;
  height: 120px;
  margin: 0 auto 0.6rem;
}
.radial {
  width: 120px;
  height: 120px;
  transform: rotate(-90deg); /* start progress from top */
}
.track {
  fill: none;
  stroke: rgba(17,24,39,0.08);
  stroke-width: 10;
}
.progress {
  fill: none;
  stroke: url(#grad) var(--primary);
  stroke: var(--primary);
  stroke-linecap: round;
  stroke-width: 10;
  transition: stroke-dashoffset .6s ease;
  filter: drop-shadow(0 6px 14px rgba(37,99,235,0.25));
}

/* Inner icon and percent overlay */
.icon {
  position: absolute;
  inset: 0;
  display: grid;
  place-items: center;
  font-size: 1.25rem;
  transform: translateY(-3px);
}
.percent {
  position: absolute;
  bottom: -6px;
  left: 50%;
  transform: translateX(-50%);
  font-weight: 700;
  font-size: 0.9rem;
  color: var(--primary);
  background: #fff;
  border: 1px solid rgba(37,99,235,0.25);
  border-radius: 999px;
  padding: 2px 8px;
  box-shadow: 0 6px 16px rgba(17,24,39,0.06);
}

/* Title under radial */
h3 {
  margin: 0.25rem 0 0;
  font-size: 0.95rem;
  font-weight: 700;
}

/* Tools/Chips */
.tools {
  margin-top: 1.25rem;
  background: var(--surface);
  border: 1px solid rgba(17,24,39,0.06);
  border-radius: 14px;
  padding: 1rem;
  box-shadow: inset 0 1px 0 rgba(255,255,255,0.35), 0 8px 24px rgba(17,24,39,0.05);
}
.tools h4 {
  margin: 0 0 0.5rem;
  font-size: 1rem;
}
.chips { display: flex; flex-wrap: wrap; gap: 0.5rem; }
.chip {
  border-radius: 999px;
  padding: 0.4rem 0.7rem;
  border: 1px solid rgba(17,24,39,0.08);
  background: linear-gradient(180deg, rgba(37,99,235,0.06), rgba(255,255,255,0.85));
  color: var(--text);
  transition: transform .1s ease, box-shadow .18s ease, border-color .18s ease;
}
.chip:hover {
  transform: translateY(-1px);
  border-color: rgba(37,99,235,0.35);
  box-shadow: 0 6px 18px rgba(17,24,39,0.08);
}
</style>
