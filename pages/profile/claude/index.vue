<template>
  <div class="portfolio">
    <!-- ============ HERO ============ -->
    <header class="hero">
      <div class="hero__grid" aria-hidden="true"></div>

      <div class="hero__top">
        <div class="status">
          <span class="status__dot"></span>
          <span class="status__text">open to new roles</span>
        </div>
        <nav class="hero__nav">
          <a href="#stack">stack</a>
          <a href="#log">log</a>
          <a href="#projects">projects</a>
          <a href="#lab">lab</a>
          <a href="#contact">contact</a>
        </nav>
      </div>

      <div class="hero__main">
        <div class="hero__mark">HTS</div>
        <div class="hero__copy">
          <h1 class="hero__name">
            Hoàng Thái Sơn
            <svg class="hero__trace" viewBox="0 0 420 24" aria-hidden="true">
              <path d="M2 20 L120 20 L140 4 L300 4 L320 20 L418 20" />
            </svg>
          </h1>
          <p class="hero__role">Full-stack engineer, backend-leaning · Spring / Java ecosystem</p>
          <p class="hero__bio">
            Five years building and operating back-end systems for banking and
            enterprise platforms — APIs, microservices, CI/CD pipelines. Lately
            spending nights on computer vision and IoT side projects, and
            planning a Ph.D. to go deeper into that research track.
          </p>

          <ul class="hero__facts">
            <li><span>exp.</span> 5+ yrs</li>
            <li><span>base</span> Hà Nội, VN</li>
            <li><span>focus</span> Backend · DevOps · Computer Vision</li>
          </ul>

          <div class="hero__actions">
            <a class="btn btn--solid" href="mailto:mikechael74@gmail.com">Email me</a>
            <a class="btn" href="tel:0327433533">0327 433 533</a>
            <a class="btn" href="https://www.facebook.com/hoangson18101998/" target="_blank" rel="noopener">Facebook ↗</a>
          </div>
        </div>
      </div>
    </header>

    <main>
      <!-- ============ STACK ============ -->
      <section id="stack" class="section">
        <div class="section__head">
          <p class="tag">// stack.arch</p>
          <h2>What the system is built from</h2>
          <p class="section__lede">
            Read top to bottom like a request would travel through it — from
            what a user sees, down to what keeps it running.
          </p>
        </div>

        <div class="stack">
          <button
            v-for="(layer, i) in skillLayers"
            :key="layer.name"
            class="stack__layer"
            :class="{ 'is-active': activeLayer === i }"
            :style="{ '--depth': i }"
            @mouseenter="activeLayer = i"
            @focus="activeLayer = i"
            @mouseleave="activeLayer = null"
            @blur="activeLayer = null"
            type="button"
          >
            <span class="stack__node"></span>
            <span class="stack__label">{{ layer.name }}</span>
            <span class="stack__items">
              <span v-for="item in layer.items" :key="item" class="chip">{{ item }}</span>
            </span>
          </button>
        </div>
      </section>

      <!-- ============ EXPERIENCE LOG ============ -->
      <section id="log" class="section">
        <div class="section__head">
          <p class="tag">// experience.log</p>
          <h2>Five years, five deploys</h2>
          <p class="section__lede">Tap an entry to expand the commit.</p>
        </div>

        <ol class="log">
          <li
            v-for="(job, i) in experience"
            :key="job.company"
            class="log__entry"
            :class="{ 'is-open': openJob === i }"
          >
            <button class="log__row" @click="toggleJob(i)" type="button" :aria-expanded="openJob === i">
              <span class="log__node">
                <span class="log__dot" :class="{ 'is-current': job.current }"></span>
              </span>
              <span class="log__hash">{{ job.hash }}</span>
              <span class="log__company">{{ job.company }}</span>
              <span class="log__role">{{ job.role }}</span>
              <span class="log__period">{{ job.period }}</span>
              <span class="log__chevron">{{ openJob === i ? '−' : '+' }}</span>
            </button>

            <div class="log__body" v-show="openJob === i">
              <div class="log__col">
                <p class="log__k">language</p>
                <p class="log__v">{{ job.language }}</p>
              </div>
              <div class="log__col">
                <p class="log__k">technology</p>
                <p class="log__v">{{ job.technology }}</p>
              </div>
              <div class="log__col">
                <p class="log__k">database</p>
                <p class="log__v">{{ job.database }}</p>
              </div>
              <div class="log__col" v-if="job.projects">
                <p class="log__k">shipped on</p>
                <p class="log__v">{{ job.projects }}</p>
              </div>
            </div>
          </li>
        </ol>
      </section>

      <!-- ============ FEATURED PROJECTS ============ -->
      <section id="projects" class="section">
        <div class="section__head">
          <p class="tag">// projects.featured</p>
          <h2>Systems shipped for real customers</h2>
        </div>

        <div class="filters">
          <button
            v-for="f in filters"
            :key="f"
            class="filters__btn"
            :class="{ 'is-active': activeFilter === f }"
            @click="activeFilter = f"
            type="button"
          >
            {{ f }}
          </button>
        </div>

        <div class="cards">
          <article v-for="p in filteredProjects" :key="p.title" class="card">
            <div class="card__meta">
              <span>{{ p.customer }}</span>
              <span>{{ p.period }}</span>
            </div>
            <h3 class="card__title">{{ p.title }}</h3>
            <p class="card__desc">{{ p.description }}</p>
            <div class="card__foot">
              <span class="card__position">{{ p.position }} · team of {{ p.member }}</span>
              <div class="card__stack">
                <span v-for="t in p.technology" :key="t" class="chip chip--ghost">{{ t }}</span>
              </div>
            </div>
          </article>
        </div>
      </section>

      <!-- ============ LAB / PERSONAL PROJECTS ============ -->
      <section id="lab" class="section section--dark">
        <div class="section__head">
          <p class="tag tag--light">// lab.personal</p>
          <h2>What gets built after hours</h2>
          <p class="section__lede section__lede--light">
            Where the backend work leaves off, computer vision and IoT pick up.
          </p>
        </div>

        <div class="lab">
          <a
            v-for="proj in personalProjects"
            :key="proj.title"
            class="lab__item"
            :href="proj.url"
            target="_blank"
            rel="noopener"
          >
            <div>
              <h3>{{ proj.title }}</h3>
              <p>{{ proj.description }}</p>
            </div>
            <span class="lab__link">github ↗</span>
          </a>
        </div>
      </section>

      <!-- ============ EDUCATION & CERTIFICATES ============ -->
      <section class="section">
        <div class="section__head">
          <p class="tag">// education.cert</p>
          <h2>Training log</h2>
        </div>

        <div class="edu">
          <div class="edu__col">
            <div v-for="e in education" :key="e.school" class="edu__item">
              <p class="edu__school">{{ e.school }}</p>
              <p class="edu__degree">{{ e.degree }}</p>
              <p class="edu__period">{{ e.period }}</p>
              <p class="edu__project">Project — {{ e.project }}</p>
            </div>
          </div>
          <div class="edu__col">
            <table class="certs">
              <tbody>
                <tr v-for="c in certificates" :key="c.name">
                  <td>{{ c.name }}</td>
                  <td class="certs__score">{{ c.score }}</td>
                  <td class="certs__year">{{ c.year }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </section>
    </main>

    <!-- ============ CONTACT / FOOTER ============ -->
    <footer id="contact" class="footer">
      <div class="footer__main">
        <h2>Let's talk about what you're building.</h2>
        <div class="footer__actions">
          <a class="btn btn--solid btn--lg" href="mailto:mikechael74@gmail.com">mikechael74@gmail.com</a>
          <a class="btn btn--lg" href="tel:0327433533">0327 433 533</a>
        </div>
      </div>
      <div class="footer__meta">
        <span>Số 17 Thiên Đức, Xã Phù Đổng, Hà Nội</span>
        <span>© {{ year }} Hoàng Thái Sơn</span>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const year = new Date().getFullYear()

/* ---------- stack layers (rendered top -> bottom like a request path) ---------- */
const skillLayers = [
  { name: 'Frontend', items: ['Vue', 'Angular', 'HTML', 'CSS / SCSS'] },
  { name: 'Framework', items: ['Spring', 'Laravel', 'NuxtJS', 'NodeJS', 'CodeIgniter 4'] },
  { name: 'Backend', items: ['Java', 'Python', 'C++', 'JavaScript', 'PHP'] },
  { name: 'Design patterns', items: ['Builder', 'Singleton', 'Observer', 'Proxy'] },
  { name: 'DevOps / infra', items: ['CI/CD', 'Jenkins', 'Docker', 'AWS', 'Rancher', 'Kibana', 'BPM', 'Tanzu'] },
]
const activeLayer = ref(null)

/* ---------- experience log ---------- */
const experience = [
  {
    hash: 'a71',
    company: 'ATI',
    role: 'Back-end Developer',
    period: '09/2025 → now',
    current: true,
    language: 'Java, JavaScript',
    technology: 'Spring Boot, JPA, RestAPI, Docker, Jenkins, CI/CD, Jira, Angular, Tanzu, BPM',
    database: 'Oracle',
  },
  {
    hash: 'f38',
    company: 'CMC Global',
    role: 'Back-end Developer',
    period: '09/2024 → 09/2025',
    language: 'Java, JavaScript',
    technology: 'Spring Boot, JPA, RestAPI, Docker, Jenkins, CI/CD, Jira',
    database: 'MySQL, Oracle',
    projects: 'Dashboard, OMS, C-NOW',
  },
  {
    hash: '9c2',
    company: 'Alphaway',
    role: 'Back-end Developer',
    period: '09/2023 → 09/2024',
    language: 'Java, JavaScript, TypeScript',
    technology: 'Spring Boot, JPA, RestAPI, gRPC, Apigee, Kibana, Docker, Jenkins, CI/CD, Angular',
    database: 'Oracle',
    projects: 'Banner Manager, Device Authentication, Expense Management, Water & Electric Bill Payment',
  },
  {
    hash: '5b0',
    company: 'Viettel Solution',
    role: 'Full-stack Developer',
    period: '06/2022 → 09/2023',
    language: 'Java, JavaScript',
    technology: 'Spring Boot, JSP, JDBC, Hibernate, Microservice, RestAPI',
    database: 'Oracle, MSSQL',
    projects: 'BCCS2 system development & maintenance, Customer Care',
  },
  {
    hash: '1a4',
    company: 'Enai Technology and Services',
    role: 'Full-stack Developer',
    period: '09/2021 → 06/2022',
    language: 'Java, PHP, JavaScript',
    technology: 'Spring Boot, Laravel, Vue, CodeIgniter 4',
    database: 'MySQL, PostgreSQL',
    projects: 'Travel platform, job-application system for Vietnamese workers in Japan',
  },
]
const openJob = ref(0)
function toggleJob(i) {
  openJob.value = openJob.value === i ? null : i
}

/* ---------- featured client projects ---------- */
const featuredProjects = [
  {
    title: 'CCTT System',
    customer: 'MB Bank',
    period: '09/2025 → now',
    description:
      'Information-provisioning system for the legal department: retrieving transaction logs, extracting entity information, and proposing implementation approaches.',
    member: 12,
    position: 'Full-stack developer',
    technology: ['Java', 'Spring', 'CI/CD', 'Docker', 'Jenkins', 'Rancher', 'Angular', 'BPM'],
  },
  {
    title: 'Office Management System',
    customer: 'CMC Global',
    period: '07/2025 → 09/2025',
    description:
      'Meeting-schedule management feature built for mobile and Smart TV, letting organizers and participants track meeting info across both platforms.',
    member: 6,
    position: 'Back-end developer',
    technology: ['Java', 'Spring', 'MySQL', 'CI/CD', 'Docker', 'Jenkins', 'Flutter'],
  },
  {
    title: 'Water & Electric Bill Payment',
    customer: 'MB Bank',
    period: '06/2024 → 09/2024',
    description:
      'Enterprise billing flow letting customers carry balances across months and settle multiple periods in one payment, integrated with payment-order approval systems.',
    member: 4,
    position: 'Back-end developer',
    technology: ['Java', 'Spring', 'Oracle', 'Apigee', 'CI/CD', 'Kibana', 'Docker', 'Kafka', 'MongoDB'],
  },
  {
    title: 'Banner Management',
    customer: 'MB Bank',
    period: '02/2024 → 06/2024',
    description:
      'Standalone banner-management service for MB websites: image banners, sliders, expiry rules, and placement, updated centrally across sites.',
    member: 4,
    position: 'Back-end developer',
    technology: ['Java', 'Spring', 'Oracle', 'Apigee', 'CI/CD', 'Kibana', 'Docker', 'Kafka', 'MongoDB'],
  },
]
const filters = ['All', 'MB Bank', 'CMC Global']
const activeFilter = ref('All')
const filteredProjects = computed(() =>
  activeFilter.value === 'All'
    ? featuredProjects
    : featuredProjects.filter((p) => p.customer === activeFilter.value)
)

/* ---------- personal lab projects ---------- */
const personalProjects = [
  {
    title: 'Online drumming system',
    description: 'AI-assisted coding experiment: a browser drum machine built with AI pair-programming.',
    url: 'https://github.com/sonkun-game/khan',
  },
  {
    title: 'Smart traffic light management',
    description: 'Computer vision + IoT circuit project adapting traffic-light timing to real traffic flow.',
    url: 'https://github.com/sonkun-game/traffict-light-control',
  },
  {
    title: 'Quiz',
    description: 'A small quiz application built to try out a new stack end to end.',
    url: 'https://github.com/sonkun-game/sunquiz',
  },
]

/* ---------- education & certificates ---------- */
const education = [
  {
    school: 'FPT School of Business & Technology',
    degree: "Master of Software Engineering",
    period: '09/2023 → 09/2025',
    project: 'Real-Time Traffic Monitoring System Utilizing IoT Cameras and Machine Learning',
  },
  {
    school: 'FPT University',
    degree: "Bachelor's Degree",
    period: '09/2016 → 09/2021',
    project: 'Ohara — Garage Management System',
  },
]
const certificates = [
  { name: 'TOEIC Listening & Reading', score: '775 / 990', year: '2025' },
  { name: 'TOEIC Writing & Speaking', score: '270 / 400', year: '2025' },
  { name: 'Vstep', score: 'B2', year: '2025' },
]
</script>

<style scoped>
/* Space Grotesk for display, Inter for body, IBM Plex Mono for technical labels */
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500&display=swap');

.portfolio {
  --paper: #efece3;
  --surface: #faf8f2;
  --ink: #1e2620;
  --ink-soft: #4c5850;
  --line: #c9c2b0;
  --accent: #3b6e5e;
  --accent-soft: #dbe6df;
  --copper: #b5602c;
  --dark: #1a221d;

  background: var(--paper);
  color: var(--ink);
  font-family: 'Inter', sans-serif;
  line-height: 1.55;
}

.portfolio * {
  box-sizing: border-box;
}

.mono {
  font-family: 'IBM Plex Mono', monospace;
}

a {
  color: inherit;
}

/* ---------- shared section shell ---------- */
.section {
  max-width: 880px;
  margin: 0 auto;
  padding: 88px 24px;
  border-bottom: 1px solid var(--line);
}

.section__head {
  max-width: 620px;
  margin-bottom: 40px;
}

.tag {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 13px;
  color: var(--accent);
  margin: 0 0 14px;
}

.tag--light {
  color: #8fd6b3;
}

.section h2 {
  font-family: 'Space Grotesk', sans-serif;
  font-size: clamp(26px, 3.4vw, 34px);
  font-weight: 600;
  margin: 0 0 10px;
  letter-spacing: -0.01em;
}

.section__lede {
  color: var(--ink-soft);
  margin: 0;
  max-width: 46ch;
}

.section__lede--light {
  color: #b9c4bd;
}

/* ---------- hero ---------- */
.hero {
  position: relative;
  max-width: 880px;
  margin: 0 auto;
  padding: 40px 24px 96px;
  overflow: hidden;
}

.hero__grid {
  position: absolute;
  inset: 0;
  background-image: linear-gradient(var(--line) 1px, transparent 1px),
    linear-gradient(90deg, var(--line) 1px, transparent 1px);
  background-size: 34px 34px;
  opacity: 0.35;
  mask-image: linear-gradient(to bottom, black, transparent 85%);
  pointer-events: none;
}

.hero__top {
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 72px;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 13px;
}

.status {
  display: flex;
  align-items: center;
  gap: 8px;
}

.status__dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--accent);
  box-shadow: 0 0 0 0 rgba(59, 110, 94, 0.5);
  animation: pulse 2.4s ease-out infinite;
}

@keyframes pulse {
  0% { box-shadow: 0 0 0 0 rgba(59, 110, 94, 0.45); }
  70% { box-shadow: 0 0 0 8px rgba(59, 110, 94, 0); }
  100% { box-shadow: 0 0 0 0 rgba(59, 110, 94, 0); }
}

.hero__nav {
  display: flex;
  gap: 20px;
}

.hero__nav a {
  text-decoration: none;
  color: var(--ink-soft);
  transition: color 0.15s ease;
}

.hero__nav a:hover,
.hero__nav a:focus-visible {
  color: var(--ink);
}

.hero__main {
  position: relative;
  display: grid;
  grid-template-columns: 96px 1fr;
  gap: 32px;
  align-items: start;
}

.hero__mark {
  width: 96px;
  height: 96px;
  border-radius: 50%;
  border: 1px solid var(--line);
  background: var(--surface);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Space Grotesk', sans-serif;
  font-weight: 600;
  font-size: 22px;
  letter-spacing: 0.02em;
  color: var(--accent);
}

.hero__name {
  position: relative;
  font-family: 'Space Grotesk', sans-serif;
  font-size: clamp(38px, 6vw, 58px);
  font-weight: 700;
  letter-spacing: -0.015em;
  margin: 0 0 10px;
  display: inline-block;
}

.hero__trace {
  display: block;
  width: 100%;
  height: 14px;
  margin-top: 4px;
  overflow: visible;
}

.hero__trace path {
  fill: none;
  stroke: var(--copper);
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-dasharray: 460;
  stroke-dashoffset: 460;
  animation: draw 1.4s 0.3s ease forwards;
}

@keyframes draw {
  to { stroke-dashoffset: 0; }
}

.hero__role {
  font-size: 17px;
  color: var(--accent);
  font-weight: 500;
  margin: 0 0 18px;
}

.hero__bio {
  max-width: 52ch;
  color: var(--ink-soft);
  margin: 0 0 26px;
}

.hero__facts {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  gap: 24px;
  padding: 0;
  margin: 0 0 32px;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 13px;
}

.hero__facts li span {
  display: block;
  color: var(--ink-soft);
  font-size: 11px;
  margin-bottom: 2px;
}

.hero__actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.btn {
  display: inline-flex;
  align-items: center;
  padding: 11px 18px;
  border-radius: 999px;
  border: 1px solid var(--ink);
  text-decoration: none;
  font-size: 14px;
  font-weight: 500;
  color: var(--ink);
  transition: transform 0.15s ease, background 0.15s ease;
}

.btn:hover {
  transform: translateY(-1px);
}

.btn--solid {
  background: var(--ink);
  color: var(--surface);
}

.btn--solid:hover {
  background: var(--accent);
  border-color: var(--accent);
}

.btn--lg {
  padding: 14px 22px;
  font-size: 15px;
}

/* ---------- stack (architecture layers) ---------- */
.stack {
  display: flex;
  flex-direction: column;
  border-left: 2px solid var(--line);
  padding-left: 22px;
}

.stack__layer {
  all: unset;
  cursor: pointer;
  position: relative;
  display: grid;
  grid-template-columns: 150px 1fr;
  gap: 16px;
  align-items: center;
  padding: 16px 0;
  border-bottom: 1px solid var(--line);
}

.stack__layer:last-child {
  border-bottom: none;
}

.stack__node {
  position: absolute;
  left: -28px;
  top: 50%;
  transform: translateY(-50%);
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--surface);
  border: 2px solid var(--line);
  transition: border-color 0.15s ease, background 0.15s ease;
}

.stack__layer.is-active .stack__node {
  border-color: var(--accent);
  background: var(--accent);
}

.stack__label {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 13px;
  color: var(--ink-soft);
}

.stack__layer.is-active .stack__label {
  color: var(--accent);
}

.stack__items {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.chip {
  font-size: 13px;
  padding: 5px 11px;
  border-radius: 999px;
  background: var(--surface);
  border: 1px solid var(--line);
  color: var(--ink);
  white-space: nowrap;
}

.chip--ghost {
  background: transparent;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 12px;
}

/* ---------- experience log ---------- */
.log {
  list-style: none;
  margin: 0;
  padding: 0;
  border-left: 2px solid var(--line);
}

.log__entry {
  position: relative;
  padding-left: 22px;
  border-bottom: 1px solid var(--line);
}

.log__entry:last-child {
  border-bottom: none;
}

.log__row {
  all: unset;
  cursor: pointer;
  width: calc(100% - 22px);
  display: grid;
  grid-template-columns: 20px 46px 1fr auto auto 18px;
  gap: 14px;
  align-items: center;
  padding: 18px 0;
}

.log__node {
  position: absolute;
  left: -7px;
  top: 26px;
}

.log__dot {
  display: block;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--surface);
  border: 2px solid var(--line);
}

.log__dot.is-current {
  border-color: var(--accent);
  background: var(--accent);
}

.log__hash {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 12px;
  color: var(--copper);
}

.log__company {
  font-weight: 600;
  font-family: 'Space Grotesk', sans-serif;
  font-size: 16px;
}

.log__role {
  color: var(--ink-soft);
  font-size: 13px;
  display: none;
}

.log__period {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 12px;
  color: var(--ink-soft);
  white-space: nowrap;
}

.log__chevron {
  font-size: 18px;
  color: var(--ink-soft);
  text-align: center;
}

.log__body {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 18px;
  padding: 0 0 24px;
}

.log__k {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
  color: var(--accent);
  margin: 0 0 4px;
}

.log__v {
  margin: 0;
  font-size: 14px;
  color: var(--ink-soft);
}

@media (min-width: 640px) {
  .log__role {
    display: block;
  }
}

/* ---------- filters + cards ---------- */
.filters {
  display: flex;
  gap: 10px;
  margin-bottom: 24px;
}

.filters__btn {
  all: unset;
  cursor: pointer;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 12px;
  padding: 8px 14px;
  border-radius: 999px;
  border: 1px solid var(--line);
  color: var(--ink-soft);
}

.filters__btn.is-active {
  border-color: var(--accent);
  color: var(--accent);
  background: var(--accent-soft);
}

.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 18px;
}

.card {
  background: var(--surface);
  border: 1px solid var(--line);
  border-radius: 4px;
  padding: 22px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.card__meta {
  display: flex;
  justify-content: space-between;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 11px;
  color: var(--accent);
}

.card__title {
  font-family: 'Space Grotesk', sans-serif;
  font-size: 18px;
  margin: 0;
}

.card__desc {
  font-size: 14px;
  color: var(--ink-soft);
  margin: 0;
  flex-grow: 1;
}

.card__foot {
  border-top: 1px solid var(--line);
  padding-top: 12px;
}

.card__position {
  display: block;
  font-size: 12px;
  color: var(--ink-soft);
  margin-bottom: 8px;
}

.card__stack {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

/* ---------- lab (dark section) ---------- */
.section--dark {
  background: var(--dark);
  color: #e9ece9;
  max-width: none;
  border-bottom: none;
}

.section--dark .section__head,
.section--dark .lab {
  max-width: 880px;
  margin-left: auto;
  margin-right: auto;
}

.lab {
  display: grid;
  gap: 1px;
  background: #2c352d;
  border: 1px solid #2c352d;
}

.lab__item {
  background: var(--dark);
  padding: 22px 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  text-decoration: none;
  color: inherit;
  transition: background 0.15s ease;
}

.lab__item:hover {
  background: #212b23;
}

.lab__item h3 {
  font-family: 'Space Grotesk', sans-serif;
  font-size: 17px;
  margin: 0 0 4px;
}

.lab__item p {
  margin: 0;
  font-size: 13px;
  color: #a9b4ac;
  max-width: 50ch;
}

.lab__link {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 12px;
  color: #8fd6b3;
  white-space: nowrap;
}

/* ---------- education / certs ---------- */
.edu {
  display: grid;
  grid-template-columns: 1.3fr 1fr;
  gap: 40px;
}

.edu__item {
  padding: 18px 0;
  border-bottom: 1px solid var(--line);
}

.edu__item:last-child {
  border-bottom: none;
}

.edu__school {
  font-family: 'Space Grotesk', sans-serif;
  font-weight: 600;
  margin: 0 0 2px;
}

.edu__degree {
  margin: 0 0 2px;
  color: var(--accent);
  font-size: 14px;
}

.edu__period {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 12px;
  color: var(--ink-soft);
  margin: 0 0 8px;
}

.edu__project {
  margin: 0;
  font-size: 13px;
  color: var(--ink-soft);
}

.certs {
  width: 100%;
  border-collapse: collapse;
  font-size: 14px;
}

.certs td {
  padding: 12px 0;
  border-bottom: 1px solid var(--line);
}

.certs__score {
  font-family: 'IBM Plex Mono', monospace;
  color: var(--accent);
  text-align: right;
}

.certs__year {
  text-align: right;
  color: var(--ink-soft);
  padding-left: 16px;
  width: 50px;
}

/* ---------- footer ---------- */
.footer {
  max-width: 880px;
  margin: 0 auto;
  padding: 88px 24px 56px;
}

.footer__main h2 {
  font-family: 'Space Grotesk', sans-serif;
  font-size: clamp(26px, 4vw, 38px);
  max-width: 14ch;
  margin: 0 0 28px;
}

.footer__actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  margin-bottom: 56px;
}

.footer__meta {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 8px;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 12px;
  color: var(--ink-soft);
  border-top: 1px solid var(--line);
  padding-top: 20px;
}

/* ---------- responsive ---------- */
@media (max-width: 640px) {
  .hero__main {
    grid-template-columns: 1fr;
  }
  .stack__layer {
    grid-template-columns: 1fr;
    gap: 8px;
  }
  .log__row {
    grid-template-columns: 16px 1fr auto 18px;
  }
  .log__hash {
    display: none;
  }
  .edu {
    grid-template-columns: 1fr;
  }
}

@media (prefers-reduced-motion: reduce) {
  .status__dot,
  .hero__trace path {
    animation: none;
  }
}
</style>