<template>
    <div class="portfolio" :class="{ 'menu-open': menuOpen }">
        <!-- Ambient background -->
        <div class="ambient ambient-1"></div>
        <div class="ambient ambient-2"></div>
        <div class="grid-bg"></div>
        <div class="noise"></div>

        <!-- Navigation -->
        <header class="nav-wrap">
            <nav class="nav">
                <a href="#home" class="brand" @click="closeMenu">
                    <span class="brand-mark">HS</span>
                    <span class="brand-text">HOÀNG THÁI SƠN<span>.</span></span>
                </a>

                <button class="menu-btn" aria-label="Toggle menu" @click="menuOpen = !menuOpen">
                    <span></span><span></span><span></span>
                </button>

                <div class="nav-links" :class="{ open: menuOpen }">
                    <a href="#about" @click="closeMenu">About</a>
                    <a href="#stack" @click="closeMenu">Stack</a>
                    <a href="#experience" @click="closeMenu">Experience</a>
                    <a href="#projects" @click="closeMenu">Projects</a>
                    <a href="#research" @click="closeMenu">Research</a>
                    <a href="#contact" class="nav-cta" @click="closeMenu">Let's talk ↗</a>
                </div>
            </nav>
        </header>

        <main>
            <!-- HERO -->
            <section id="home" class="hero section">
                <div class="hero-copy">
                    <div class="eyebrow">
                        <span class="status-dot"></span>
                        FULL STACK DEVELOPER · AI / COMPUTER VISION
                    </div>

                    <h1>
                        I build
                        <span class="gradient-text">systems</span>
                        that connect
                        <span class="outline-text">code</span>
                        to the real world.
                    </h1>

                    <p class="hero-description">
                        Results-driven software engineer with 5+ years across backend,
                        frontend, DevOps and AI. I turn complex ideas into reliable,
                        scalable products — and I am especially interested in
                        Computer Vision, Machine Learning and intelligent systems.
                    </p>

                    <div class="hero-actions">
                        <a href="#projects" class="btn btn-primary">
                            Explore my work <span>↗</span>
                        </a>
                        <a href="#contact" class="btn btn-ghost">
                            Contact me
                        </a>
                    </div>

                    <div class="hero-meta">
                        <div>
                            <strong>5+</strong>
                            <span>Years in tech</span>
                        </div>
                        <div>
                            <strong>FULL</strong>
                            <span>Stack mindset</span>
                        </div>
                        <div>
                            <strong>AI</strong>
                            <span>Research focus</span>
                        </div>
                    </div>
                </div>

                <!-- Interactive visual -->
                <div class="hero-visual">
                    <div class="orbit orbit-a"></div>
                    <div class="orbit orbit-b"></div>
                    <div class="orbit orbit-c"></div>

                    <div class="code-card">
                        <div class="window-bar">
                            <span></span><span></span><span></span>
                            <small>son@portfolio: ~</small>
                        </div>
                        <div class="terminal">
                            <p><i>$</i> whoami</p>
                            <h3>hoang_thai_son</h3>
                            <p><i>$</i> mission</p>
                            <div class="terminal-line">
                                <span>build(</span>
                                <b>intelligent_systems</b>
                                <span>)</span>
                            </div>
                            <p><i>$</i> focus</p>
                            <div class="tags">
                                <span>Java</span>
                                <span>Spring</span>
                                <span>Vue</span>
                                <span>Docker</span>
                                <span>AI</span>
                            </div>
                            <p><i>$</i> status <span class="success">● ONLINE</span></p>
                        </div>
                    </div>

                    <div class="floating-chip chip-java">JAVA</div>
                    <div class="floating-chip chip-ai">AI</div>
                    <div class="floating-chip chip-vue">VUE</div>
                    <div class="floating-chip chip-docker">DOCKER</div>
                </div>
            </section>

            <!-- ABOUT -->
            <section id="about" class="section about">
                <div class="section-label">01 / ABOUT</div>
                <div class="section-heading">
                    <h2>Engineer by trade.<br /><span>Researcher by curiosity.</span></h2>
                    <p>
                        My career has moved from full-stack product development toward
                        larger distributed systems, cloud infrastructure and AI-powered
                        applications.
                    </p>
                </div>

                <div class="about-grid">
                    <article class="manifesto-card">
                        <div class="card-number">01</div>
                        <h3>Build.</h3>
                        <p>
                            Backend architecture, REST APIs, microservices, databases,
                            frontend interfaces and CI/CD pipelines — I enjoy understanding
                            the whole system rather than only one layer.
                        </p>
                    </article>

                    <article class="manifesto-card featured">
                        <div class="card-number">02</div>
                        <h3>Connect.</h3>
                        <p>
                            My favorite problems sit at the intersection of software and the
                            physical world: IoT, cameras, computer vision, edge devices,
                            sensors and intelligent automation.
                        </p>
                    </article>

                    <article class="manifesto-card">
                        <div class="card-number">03</div>
                        <h3>Research.</h3>
                        <p>
                            I completed a Master of Software Engineering with a project on
                            real-time traffic monitoring using IoT cameras and Machine
                            Learning, and I plan to continue toward Ph.D.-level research.
                        </p>
                    </article>
                </div>
            </section>

            <!-- STACK -->
            <section id="stack" class="section stack-section">
                <div class="section-label">02 / TECH STACK</div>
                <div class="section-heading row-heading">
                    <div>
                        <h2>Tools I use to<br /><span>make things happen.</span></h2>
                    </div>
                    <p>
                        A practical stack shaped by enterprise systems, full-stack products,
                        DevOps and experimentation with AI.
                    </p>
                </div>

                <div class="stack-wall">
                    <div v-for="(group, index) in skillGroups" :key="group.title" class="skill-group"
                        :class="{ wide: index === 0 }">
                        <div class="skill-icon">{{ group.icon }}</div>
                        <div>
                            <span class="skill-group-title">{{ group.title }}</span>
                            <div class="skill-list">
                                <span v-for="skill in group.skills" :key="skill">{{ skill }}</span>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- EXPERIENCE -->
            <section id="experience" class="section experience">
                <div class="section-label">03 / EXPERIENCE</div>
                <div class="section-heading row-heading">
                    <div>
                        <h2>A timeline of<br /><span>shipping software.</span></h2>
                    </div>
                    <p>
                        From full-stack development to enterprise backend systems and
                        distributed services.
                    </p>
                </div>

                <div class="timeline">
                    <div v-for="(job, index) in experience" :key="job.company" class="timeline-item">
                        <div class="timeline-marker">
                            <span>{{ String(index + 1).padStart(2, '0') }}</span>
                        </div>

                        <div class="timeline-content">
                            <div class="timeline-top">
                                <span class="date">{{ job.period }}</span>
                                <span class="role">{{ job.role }}</span>
                            </div>
                            <h3>{{ job.company }}</h3>
                            <p class="job-summary">{{ job.summary }}</p>

                            <div class="tech-row">
                                <span v-for="tech in job.tech" :key="tech">{{ tech }}</span>
                            </div>

                            <div v-if="job.projects" class="project-note">
                                <span>PROJECTS</span> {{ job.projects }}
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- FEATURED PROJECTS -->
            <section id="projects" class="section projects">
                <div class="section-label">04 / SELECTED WORK</div>
                <div class="section-heading">
                    <h2>Projects where<br /><span>software meets reality.</span></h2>
                </div>

                <div class="project-grid">
                    <article v-for="(project, index) in projects" :key="project.title" class="project-card"
                        :class="`project-${index + 1}`">
                        <div class="project-glow"></div>
                        <div class="project-top">
                            <span class="project-index">0{{ index + 1 }}</span>
                            <span class="project-type">{{ project.type }}</span>
                        </div>

                        <div class="project-art" :class="project.art">
                            <template v-if="project.art === 'traffic'">
                                <div class="road road-h"></div>
                                <div class="road road-v"></div>
                                <div class="traffic-light tl-1"><b></b></div>
                                <div class="traffic-light tl-2"><b></b></div>
                                <div class="car car-1"></div>
                                <div class="car car-2"></div>
                                <div class="camera">◉</div>
                            </template>

                            <template v-else-if="project.art === 'drum'">
                                <div class="drum-circle"></div>
                                <div class="drum-stick stick-1"></div>
                                <div class="drum-stick stick-2"></div>
                                <div class="sound-wave wave-1"></div>
                                <div class="sound-wave wave-2"></div>
                                <div class="sound-wave wave-3"></div>
                            </template>

                            <template v-else>
                                <div class="quiz-screen">
                                    <span>?</span>
                                    <i></i><i></i><i></i>
                                </div>
                            </template>
                        </div>

                        <div class="project-body">
                            <h3>{{ project.title }}</h3>
                            <p>{{ project.description }}</p>

                            <div class="project-tags">
                                <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
                            </div>

                            <a v-if="project.url" :href="project.url" target="_blank" rel="noopener noreferrer"
                                class="project-link">
                                View project <span>↗</span>
                            </a>
                        </div>
                    </article>
                </div>
            </section>

            <!-- RESEARCH -->
            <section id="research" class="section research">
                <div class="research-layout">
                    <div>
                        <div class="section-label">05 / RESEARCH</div>
                        <h2>
                            <span class="small">Master's Project</span>
                            Real-Time Traffic<br />
                            <em>Monitoring System</em>
                        </h2>
                        <p class="research-copy">
                            Utilizing IoT Cameras and Machine Learning to explore intelligent
                            traffic monitoring and signal control. The project combines
                            computer vision with physical hardware to create a real-time
                            traffic management prototype.
                        </p>

                        <div class="research-stack">
                            <span>Computer Vision</span>
                            <span>Machine Learning</span>
                            <span>YOLO</span>
                            <span>IoT</span>
                            <span>Raspberry Pi</span>
                        </div>
                    </div>

                    <div class="research-machine">
                        <div class="machine-ring ring-1"></div>
                        <div class="machine-ring ring-2"></div>
                        <div class="machine-core">
                            <div class="core-eye">◉</div>
                            <span>VISION</span>
                            <strong>01</strong>
                        </div>

                        <div class="data-pill p1">CAMERA / INPUT</div>
                        <div class="data-pill p2">OBJECT DETECTION</div>
                        <div class="data-pill p3">DENSITY → CONTROL</div>
                        <div class="data-pill p4">GPIO / LED</div>
                    </div>
                </div>
            </section>

            <!-- EDUCATION -->
            <section class="section education">
                <div class="section-label">06 / EDUCATION</div>
                <div class="education-grid">
                    <div>
                        <h2>Learning never<br /><span>stops compiling.</span></h2>
                    </div>

                    <div class="education-list">
                        <article>
                            <span>2023 — 2025</span>
                            <h3>Master of Software Engineering</h3>
                            <p>FPT School of Business & Technology</p>
                            <small>Project: Real-Time Traffic Monitoring System Utilizing IoT Cameras and Machine
                                Learning</small>
                        </article>

                        <article>
                            <span>2016 — 2021</span>
                            <h3>FPT University Bachelor's Degree</h3>
                            <p>FPT University</p>
                            <small>Project: Ohara — Garage Management System</small>
                        </article>

                        <article>
                            <span>2025</span>
                            <h3>English Certifications</h3>
                            <p>TOEIC · VSTEP</p>
                            <small>TOEIC L&amp;R 775/990 · TOEIC W&amp;S 270/400 · VSTEP B2</small>
                        </article>
                    </div>
                </div>
            </section>

            <!-- CONTACT -->
            <section id="contact" class="section contact">
                <div class="contact-card">
                    <div class="contact-orb"></div>

                    <div class="section-label">07 / CONTACT</div>
                    <h2>Have an idea?<br /><span>Let's build it.</span></h2>
                    <p>
                        Open to interesting software, AI, Computer Vision, research and
                        technology-development opportunities.
                    </p>

                    <div class="contact-actions">
                        <a href="mailto:mikechael74@gmail.com" class="btn btn-primary">
                            mikechael74@gmail.com ↗
                        </a>
                        <a href="https://github.com/sonkun-game" target="_blank" rel="noopener noreferrer"
                            class="btn btn-ghost">
                            GitHub ↗
                        </a>
                    </div>

                    <div class="contact-footer">
                        <span>HANOI, VIETNAM</span>
                        <span>HOÀNG THÁI SƠN © {{ currentYear }}</span>
                    </div>
                </div>
            </section>
        </main>

        <footer class="footer">
            <span>DESIGNED / CODED WITH CURIOSITY</span>
            <span>FULL STACK · AI · COMPUTER VISION · DEVOPS</span>
        </footer>
    </div>
</template>

<script setup>
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'

const menuOpen = ref(false)
const currentYear = computed(() => new Date().getFullYear())

const closeMenu = () => {
    menuOpen.value = false
}

const skillGroups = [
    {
        title: 'BACK-END',
        icon: '01',
        skills: ['Java', 'Python', 'C++', 'JavaScript', 'PHP']
    },
    {
        title: 'FRONT-END',
        icon: '02',
        skills: ['Vue', 'Angular', 'NuxtJS', 'HTML', 'CSS / SCSS']
    },
    {
        title: 'FRAMEWORKS',
        icon: '03',
        skills: ['Spring', 'Spring Boot', 'Laravel', 'NodeJS', 'CodeIgniter 4']
    },
    {
        title: 'DEVOPS',
        icon: '04',
        skills: ['CI/CD', 'Jenkins', 'Docker', 'AWS', 'Rancher', 'Kibana', 'Tanzu']
    },
    {
        title: 'ARCHITECTURE',
        icon: '05',
        skills: ['REST API', 'JPA', 'Hibernate', 'Microservice', 'gRPC', 'Design Patterns']
    },
    {
        title: 'DATA',
        icon: '06',
        skills: ['Oracle', 'MySQL', 'PostgreSQL', 'MSSQL', 'MongoDB', 'Kafka']
    }
]

const experience = [
    {
        period: '09/2025 — NOW',
        company: 'ATI',
        role: 'Back-end Developer',
        summary: 'Developing enterprise backend services and information-provisioning systems with Java and modern delivery infrastructure.',
        tech: ['Java', 'Spring Boot', 'JPA', 'REST API', 'Docker', 'Jenkins', 'Angular', 'Rancher', 'BPM'],
        projects: 'MB Bank — CCTT System'
    },
    {
        period: '09/2024 — 09/2025',
        company: 'CMC GLOBAL',
        role: 'Back-end Developer',
        summary: 'Worked on enterprise systems including dashboards, OMS and C-NOW, contributing to backend services and CI/CD workflows.',
        tech: ['Java', 'Spring Boot', 'JPA', 'REST API', 'Docker', 'Jenkins', 'Jira'],
        projects: 'Dashboard, OMS, C-NOW'
    },
    {
        period: '09/2023 — 09/2024',
        company: 'ALPHAWAY',
        role: 'Back-end Developer',
        summary: 'Built backend services and integrations for business applications, including authentication and management systems.',
        tech: ['Java', 'Spring Boot', 'gRPC', 'Apigee', 'Kibana', 'Docker', 'Angular'],
        projects: 'Banner Manager, Device Authentication, Expense Management, Water & Electric Bill Payment'
    },
    {
        period: '06/2022 — 09/2023',
        company: 'VIETTEL SOLUTION',
        role: 'Full Stack Developer',
        summary: 'Developed and maintained enterprise systems using a full-stack Java ecosystem with microservices and REST APIs.',
        tech: ['Java', 'Spring Boot', 'JSP', 'JDBC', 'Hibernate', 'Microservice', 'REST API'],
        projects: 'BCCS2 System, Customer Care'
    },
    {
        period: '09/2021 — 06/2022',
        company: 'ENAI TECHNOLOGY AND SERVICES',
        role: 'Full Stack Developer',
        summary: 'Built web applications for travel and job-application workflows, working across backend and frontend layers.',
        tech: ['Java', 'PHP', 'JavaScript', 'Spring Boot', 'Laravel', 'Vue', 'CodeIgniter 4'],
        projects: 'Travel System, Job Application System for Vietnamese people in Japan'
    }
]

const projects = [
    {
        type: 'MASTER RESEARCH · AI + IOT',
        title: 'Smart Traffic Light Management',
        description: 'A real-time traffic monitoring prototype integrating Computer Vision, Machine Learning, IoT cameras and physical traffic-light control.',
        tags: ['Computer Vision', 'YOLO', 'IoT', 'Raspberry Pi'],
        art: 'traffic',
        url: 'https://github.com/sonkun-game/traffict-light-control'
    },
    {
        type: 'EXPERIMENT · AI CODING',
        title: 'Online Drumming System',
        description: 'An experimental online drumming project created with AI-assisted coding, combining interaction, timing and web technology.',
        tags: ['Web', 'AI Coding', 'Interactive'],
        art: 'drum',
        url: 'https://github.com/sonkun-game/khan'
    },
    {
        type: 'PERSONAL PROJECT',
        title: 'SunQuiz',
        description: 'A quiz-focused personal project exploring interactive application development and game-like user experiences.',
        tags: ['JavaScript', 'Quiz', 'Interactive'],
        art: 'quiz',
        url: 'https://github.com/sonkun-game/sunquiz'
    }
]

// Small pointer-reactive parallax effect.
// No library required.
const handlePointerMove = (event) => {
    const x = (event.clientX / window.innerWidth - 0.5) * 2
    const y = (event.clientY / window.innerHeight - 0.5) * 2

    document.documentElement.style.setProperty('--mx', `${x}`)
    document.documentElement.style.setProperty('--my', `${y}`)
}

onMounted(() => {
    window.addEventListener('pointermove', handlePointerMove, { passive: true })
})

onBeforeUnmount(() => {
    window.removeEventListener('pointermove', handlePointerMove)
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Manrope:wght@400;500;600;700;800&family=Space+Grotesk:wght@400;500;600;700&display=swap');

:global(html) {
    scroll-behavior: smooth;
}

:global(body) {
    margin: 0;
    background: #07090d;
    color: #f4f4f0;
    font-family: 'Manrope', sans-serif;
}

:global(*) {
    box-sizing: border-box;
}

:global(a) {
    color: inherit;
    text-decoration: none;
}

.portfolio {
    --bg: #07090d;
    --panel: #0d1118;
    --panel-2: #10151e;
    --text: #f4f4f0;
    --muted: #8d96a5;
    --line: rgba(255, 255, 255, 0.09);
    --cyan: #67e8f9;
    --green: #75f2a6;
    --violet: #9d8cff;
    --orange: #ff9f5b;
    --mx: 0;
    --my: 0;
    min-height: 100vh;
    overflow: hidden;
    background:
        radial-gradient(circle at 10% 15%, rgba(103, 232, 249, 0.06), transparent 28%),
        radial-gradient(circle at 90% 45%, rgba(157, 140, 255, 0.07), transparent 30%),
        var(--bg);
}

.ambient {
    position: fixed;
    width: 32rem;
    height: 32rem;
    border-radius: 50%;
    filter: blur(100px);
    pointer-events: none;
    z-index: 0;
    opacity: 0.11;
}

.ambient-1 {
    top: 10%;
    left: -15rem;
    background: var(--cyan);
}

.ambient-2 {
    right: -15rem;
    top: 50%;
    background: var(--violet);
}

.grid-bg {
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 0;
    opacity: 0.24;
    background-image:
        linear-gradient(rgba(255, 255, 255, 0.025) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255, 255, 255, 0.025) 1px, transparent 1px);
    background-size: 50px 50px;
    mask-image: linear-gradient(to bottom, black, transparent 85%);
}

.noise {
    position: fixed;
    inset: 0;
    z-index: 100;
    pointer-events: none;
    opacity: 0.025;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 180 180' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.8' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='.6'/%3E%3C/svg%3E");
}

.nav-wrap {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 50;
    padding: 1.25rem 3.5rem;
    background: linear-gradient(to bottom, rgba(7, 9, 13, 0.92), transparent);
    backdrop-filter: blur(8px);
}

.nav {
    max-width: 1400px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.brand {
    display: inline-flex;
    align-items: center;
    gap: 0.75rem;
    font-family: 'Space Grotesk', sans-serif;
    font-weight: 700;
    letter-spacing: -0.03em;
}

.brand-mark {
    display: grid;
    place-items: center;
    width: 38px;
    height: 38px;
    border: 1px solid rgba(103, 232, 249, 0.35);
    border-radius: 10px;
    color: var(--cyan);
    font-size: 0.7rem;
    box-shadow: inset 0 0 18px rgba(103, 232, 249, 0.08);
}

.brand-text span {
    color: var(--cyan);
}

.nav-links {
    display: flex;
    align-items: center;
    gap: 1.8rem;
    color: var(--muted);
    font-family: 'DM Mono', monospace;
    font-size: 0.72rem;
    text-transform: uppercase;
}

.nav-links a {
    transition: color 0.2s ease;
}

.nav-links a:hover {
    color: var(--text);
}

.nav-cta {
    padding: 0.75rem 1rem;
    border: 1px solid var(--line);
    border-radius: 8px;
    color: var(--text) !important;
}

.menu-btn {
    display: none;
    width: 42px;
    height: 42px;
    padding: 10px;
    border: 1px solid var(--line);
    border-radius: 9px;
    background: rgba(255, 255, 255, 0.03);
    cursor: pointer;
}

.menu-btn span {
    display: block;
    height: 1px;
    margin: 5px 0;
    background: var(--text);
}

.section {
    position: relative;
    z-index: 1;
    max-width: 1400px;
    margin: 0 auto;
    padding: 8rem 3.5rem;
}

.hero {
    min-height: 100vh;
    display: grid;
    grid-template-columns: 1.1fr 0.9fr;
    align-items: center;
    gap: 3rem;
    padding-top: 9rem;
}

.eyebrow,
.section-label,
.skill-group-title,
.project-type,
.date,
.contact-footer,
.footer {
    font-family: 'DM Mono', monospace;
    text-transform: uppercase;
    letter-spacing: 0.08em;
}

.eyebrow {
    display: flex;
    align-items: center;
    gap: 0.65rem;
    color: var(--cyan);
    font-size: 0.68rem;
    margin-bottom: 1.6rem;
}

.status-dot {
    width: 7px;
    height: 7px;
    border-radius: 50%;
    background: var(--green);
    box-shadow: 0 0 12px var(--green);
    animation: pulse 2s infinite;
}

.hero h1 {
    max-width: 900px;
    margin: 0;
    font-family: 'Space Grotesk', sans-serif;
    font-size: clamp(3.4rem, 7vw, 7.5rem);
    line-height: 0.93;
    letter-spacing: -0.065em;
    font-weight: 600;
}

.gradient-text {
    background: linear-gradient(100deg, var(--cyan), var(--violet));
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
}

.outline-text {
    color: transparent;
    -webkit-text-stroke: 1px rgba(244, 244, 240, 0.65);
}

.hero-description {
    max-width: 650px;
    margin: 2rem 0;
    color: var(--muted);
    font-size: 1rem;
    line-height: 1.9;
}

.hero-actions {
    display: flex;
    flex-wrap: wrap;
    gap: 0.8rem;
}

.btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    gap: 0.65rem;
    min-height: 48px;
    padding: 0.8rem 1.1rem;
    border-radius: 9px;
    font-family: 'DM Mono', monospace;
    font-size: 0.72rem;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    transition: transform 0.2s ease, border-color 0.2s ease, background 0.2s ease;
}

.btn:hover {
    transform: translateY(-3px);
}

.btn-primary {
    background: var(--text);
    color: #080a0e;
    border: 1px solid var(--text);
}

.btn-ghost {
    border: 1px solid var(--line);
    background: rgba(255, 255, 255, 0.025);
}

.btn-ghost:hover {
    border-color: rgba(103, 232, 249, 0.4);
}

.hero-meta {
    display: flex;
    gap: 2.5rem;
    margin-top: 3rem;
}

.hero-meta div {
    display: flex;
    flex-direction: column;
    gap: 0.2rem;
}

.hero-meta strong {
    font-family: 'Space Grotesk', sans-serif;
    font-size: 1.25rem;
}

.hero-meta span {
    color: var(--muted);
    font-family: 'DM Mono', monospace;
    font-size: 0.6rem;
    text-transform: uppercase;
}

.hero-visual {
    position: relative;
    min-height: 600px;
    display: grid;
    place-items: center;
    transform: translate(calc(var(--mx) * 8px), calc(var(--my) * 8px));
    transition: transform 0.15s ease-out;
}

.code-card {
    position: relative;
    width: min(100%, 500px);
    border: 1px solid rgba(255, 255, 255, 0.11);
    border-radius: 18px;
    background: rgba(13, 17, 24, 0.78);
    box-shadow:
        0 40px 100px rgba(0, 0, 0, 0.45),
        inset 0 0 70px rgba(103, 232, 249, 0.025);
    backdrop-filter: blur(16px);
    overflow: hidden;
    transform: rotate(calc(var(--mx) * 1deg)) translateY(calc(var(--my) * -8px));
    transition: transform 0.15s ease-out;
}

.window-bar {
    display: flex;
    align-items: center;
    gap: 6px;
    height: 44px;
    padding: 0 1rem;
    border-bottom: 1px solid var(--line);
    background: rgba(255, 255, 255, 0.02);
}

.window-bar>span {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.2);
}

.window-bar small {
    margin-left: 0.5rem;
    color: #697384;
    font: 0.62rem 'DM Mono', monospace;
}

.terminal {
    min-height: 390px;
    padding: 2rem;
    font-family: 'DM Mono', monospace;
    font-size: 0.78rem;
}

.terminal p {
    color: #677282;
    margin: 0 0 0.8rem;
}

.terminal p i {
    color: var(--green);
    font-style: normal;
    margin-right: 0.5rem;
}

.terminal h3 {
    margin: 0 0 2rem;
    color: var(--cyan);
    font-size: 1.1rem;
    font-weight: 500;
}

.terminal-line {
    margin: 0 0 2rem;
    color: #d9dee8;
}

.terminal-line b {
    color: var(--violet);
    font-weight: 500;
}

.tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-bottom: 2rem;
}

.tags span,
.project-tags span,
.research-stack span,
.tech-row span {
    border: 1px solid var(--line);
    border-radius: 999px;
    padding: 0.42rem 0.65rem;
    color: #aeb7c6;
    background: rgba(255, 255, 255, 0.025);
    font-size: 0.63rem;
}

.success {
    color: var(--green);
    margin-left: 0.5rem;
}

.orbit {
    position: absolute;
    border: 1px solid rgba(103, 232, 249, 0.12);
    border-radius: 50%;
    pointer-events: none;
}

.orbit-a {
    width: 570px;
    height: 570px;
    animation: spin 24s linear infinite;
}

.orbit-b {
    width: 430px;
    height: 430px;
    border-color: rgba(157, 140, 255, 0.12);
    animation: spin-reverse 18s linear infinite;
}

.orbit-c {
    width: 690px;
    height: 690px;
    border-style: dashed;
    opacity: 0.5;
    animation: spin 35s linear infinite;
}

.floating-chip {
    position: absolute;
    padding: 0.55rem 0.75rem;
    border: 1px solid var(--line);
    border-radius: 8px;
    background: rgba(13, 17, 24, 0.86);
    backdrop-filter: blur(10px);
    color: var(--muted);
    font: 0.62rem 'DM Mono', monospace;
    box-shadow: 0 12px 35px rgba(0, 0, 0, 0.25);
}

.chip-java {
    top: 11%;
    left: 4%;
    color: #ffad74;
    animation: float 4s ease-in-out infinite;
}

.chip-ai {
    top: 18%;
    right: 3%;
    color: var(--violet);
    animation: float 5s 0.5s ease-in-out infinite;
}

.chip-vue {
    bottom: 15%;
    left: 2%;
    color: #65e6a1;
    animation: float 4.5s 1s ease-in-out infinite;
}

.chip-docker {
    bottom: 8%;
    right: 7%;
    color: var(--cyan);
    animation: float 5.5s 0.7s ease-in-out infinite;
}

.section-label {
    color: #626b7a;
    font-size: 0.65rem;
    margin-bottom: 2.2rem;
}

.section-heading {
    display: grid;
    grid-template-columns: minmax(0, 1fr);
    gap: 1rem;
    margin-bottom: 4rem;
}

.section-heading.row-heading {
    grid-template-columns: 1fr 0.5fr;
    align-items: end;
}

.section-heading h2,
.education h2,
.contact h2 {
    margin: 0;
    font-family: 'Space Grotesk', sans-serif;
    font-size: clamp(2.6rem, 5vw, 5.5rem);
    line-height: 0.98;
    letter-spacing: -0.06em;
    font-weight: 600;
}

.section-heading h2 span,
.education h2 span,
.contact h2 span {
    color: #596171;
}

.section-heading p {
    color: var(--muted);
    line-height: 1.8;
    margin: 0;
}

.about {
    border-top: 1px solid var(--line);
}

.about-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1px;
    background: var(--line);
    border: 1px solid var(--line);
}

.manifesto-card {
    min-height: 310px;
    padding: 2rem;
    background: var(--bg);
    transition: background 0.25s ease;
}

.manifesto-card:hover {
    background: #0b0f15;
}

.manifesto-card.featured {
    background:
        radial-gradient(circle at 90% 10%, rgba(103, 232, 249, 0.1), transparent 45%),
        #0a0e14;
}

.card-number {
    color: #4f5866;
    font: 0.65rem 'DM Mono', monospace;
    margin-bottom: 5rem;
}

.manifesto-card h3 {
    margin: 0 0 1rem;
    font: 500 2rem 'Space Grotesk', sans-serif;
}

.manifesto-card p {
    margin: 0;
    max-width: 370px;
    color: var(--muted);
    line-height: 1.75;
    font-size: 0.9rem;
}

.stack-section {
    border-top: 1px solid var(--line);
}

.stack-wall {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 1px;
    background: var(--line);
    border: 1px solid var(--line);
}

.skill-group {
    display: grid;
    grid-template-columns: 50px 1fr;
    gap: 1.5rem;
    min-height: 150px;
    padding: 1.7rem;
    background: var(--bg);
}

.skill-group:hover {
    background: #0b0f15;
}

.skill-icon {
    display: grid;
    place-items: center;
    width: 42px;
    height: 42px;
    border: 1px solid var(--line);
    border-radius: 50%;
    color: var(--cyan);
    font: 0.62rem 'DM Mono', monospace;
}

.skill-group-title {
    display: block;
    margin: 0.2rem 0 1rem;
    color: #697383;
    font-size: 0.62rem;
}

.skill-list {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
}

.skill-list span {
    color: #d4dae3;
    font-family: 'Space Grotesk', sans-serif;
    font-size: 0.88rem;
}

.skill-list span:not(:last-child)::after {
    content: ' /';
    color: #454d59;
    margin-left: 0.45rem;
}

.experience {
    border-top: 1px solid var(--line);
}

.timeline {
    position: relative;
    margin-left: 1rem;
}

.timeline::before {
    content: '';
    position: absolute;
    top: 0;
    bottom: 0;
    left: 22px;
    width: 1px;
    background: linear-gradient(to bottom, var(--cyan), rgba(255, 255, 255, 0.06), transparent);
}

.timeline-item {
    position: relative;
    display: grid;
    grid-template-columns: 75px 1fr;
    gap: 2rem;
    padding-bottom: 4rem;
}

.timeline-marker {
    position: relative;
    z-index: 2;
    display: grid;
    place-items: center;
    width: 45px;
    height: 45px;
    border: 1px solid rgba(103, 232, 249, 0.3);
    border-radius: 50%;
    background: var(--bg);
    color: var(--cyan);
    font: 0.6rem 'DM Mono', monospace;
}

.timeline-content {
    padding: 1.6rem;
    border: 1px solid var(--line);
    border-radius: 12px;
    background: rgba(13, 17, 24, 0.52);
    transition: transform 0.2s ease, border-color 0.2s ease;
}

.timeline-content:hover {
    transform: translateX(8px);
    border-color: rgba(103, 232, 249, 0.22);
}

.timeline-top {
    display: flex;
    justify-content: space-between;
    gap: 1rem;
    margin-bottom: 0.7rem;
}

.date {
    color: var(--cyan);
    font-size: 0.62rem;
}

.role {
    color: #626b7a;
    font: 0.62rem 'DM Mono', monospace;
    text-transform: uppercase;
}

.timeline-content h3 {
    margin: 0 0 0.8rem;
    font: 500 1.7rem 'Space Grotesk', sans-serif;
}

.job-summary {
    max-width: 800px;
    margin: 0 0 1.25rem;
    color: var(--muted);
    line-height: 1.75;
    font-size: 0.88rem;
}

.tech-row,
.project-tags,
.research-stack {
    display: flex;
    flex-wrap: wrap;
    gap: 0.45rem;
}

.project-note {
    margin-top: 1.2rem;
    padding-top: 1rem;
    border-top: 1px solid var(--line);
    color: #9ca5b3;
    font-size: 0.78rem;
    line-height: 1.6;
}

.project-note span {
    color: #596171;
    font: 0.6rem 'DM Mono', monospace;
    margin-right: 0.6rem;
}

.projects {
    border-top: 1px solid var(--line);
}

.project-grid {
    display: grid;
    grid-template-columns: 1.2fr 0.8fr;
    gap: 1rem;
}

.project-card {
    position: relative;
    min-height: 600px;
    overflow: hidden;
    border: 1px solid var(--line);
    border-radius: 15px;
    background: #0a0e14;
    transition: transform 0.3s ease, border-color 0.3s ease;
}

.project-card:hover {
    transform: translateY(-8px);
    border-color: rgba(255, 255, 255, 0.17);
}

.project-1 {
    grid-row: span 2;
}

.project-glow {
    position: absolute;
    width: 350px;
    height: 350px;
    top: -180px;
    right: -150px;
    border-radius: 50%;
    background: var(--cyan);
    filter: blur(100px);
    opacity: 0.06;
}

.project-top {
    position: relative;
    z-index: 3;
    display: flex;
    justify-content: space-between;
    padding: 1.4rem;
}

.project-index {
    color: #5d6674;
    font: 0.65rem 'DM Mono', monospace;
}

.project-type {
    color: #6e7786;
    font-size: 0.58rem;
}

.project-art {
    position: relative;
    height: 280px;
    margin: 0 1rem;
    overflow: hidden;
    border: 1px solid rgba(255, 255, 255, 0.06);
    border-radius: 11px;
    background: #070a0f;
}

.project-1 .project-art {
    height: 390px;
}

.project-body {
    position: relative;
    z-index: 2;
    padding: 1.5rem;
}

.project-body h3 {
    margin: 0 0 0.7rem;
    font: 500 1.7rem 'Space Grotesk', sans-serif;
}

.project-body p {
    margin: 0 0 1.2rem;
    color: var(--muted);
    line-height: 1.7;
    font-size: 0.82rem;
}

.project-link {
    display: inline-flex;
    gap: 0.4rem;
    margin-top: 1.1rem;
    color: var(--cyan);
    font: 0.68rem 'DM Mono', monospace;
    text-transform: uppercase;
}

.project-link:hover span {
    transform: translate(3px, -3px);
}

.project-link span {
    transition: transform 0.2s ease;
}

.traffic {
    background:
        radial-gradient(circle at 30% 30%, rgba(103, 232, 249, 0.08), transparent 30%),
        #05080c;
}

.road {
    position: absolute;
    background: #171d26;
}

.road-h {
    top: 45%;
    left: 0;
    width: 100%;
    height: 28%;
    border-top: 1px dashed rgba(255, 255, 255, 0.13);
    border-bottom: 1px dashed rgba(255, 255, 255, 0.13);
}

.road-v {
    top: 0;
    left: 45%;
    width: 28%;
    height: 100%;
    border-left: 1px dashed rgba(255, 255, 255, 0.13);
    border-right: 1px dashed rgba(255, 255, 255, 0.13);
}

.traffic-light {
    position: absolute;
    width: 13px;
    height: 35px;
    border: 1px solid #48515e;
    border-radius: 4px;
    background: #0d1117;
    box-shadow: 0 0 15px rgba(103, 232, 249, 0.1);
}

.traffic-light b {
    display: block;
    width: 5px;
    height: 5px;
    margin: 6px auto;
    border-radius: 50%;
    background: var(--green);
    box-shadow: 0 0 8px var(--green);
}

.tl-1 {
    top: 36%;
    left: 39%;
}

.tl-2 {
    bottom: 28%;
    right: 31%;
}

.car {
    position: absolute;
    width: 28px;
    height: 12px;
    border: 1px solid rgba(103, 232, 249, 0.45);
    border-radius: 3px;
    background: rgba(103, 232, 249, 0.1);
}

.car-1 {
    top: 53%;
    left: -30px;
    animation: drive 5s linear infinite;
}

.car-2 {
    left: 56%;
    top: -30px;
    transform: rotate(90deg);
    animation: drive-v 5.5s 1s linear infinite;
}

.camera {
    position: absolute;
    right: 13%;
    top: 14%;
    color: var(--cyan);
    font-size: 1.2rem;
    animation: blink 2s infinite;
}

.drum {
    display: grid;
    place-items: center;
    background:
        radial-gradient(circle at center, rgba(157, 140, 255, 0.15), transparent 30%),
        #07090e;
}

.drum-circle {
    width: 125px;
    height: 125px;
    border: 1px solid rgba(157, 140, 255, 0.55);
    border-radius: 50%;
    box-shadow:
        0 0 0 20px rgba(157, 140, 255, 0.04),
        0 0 0 40px rgba(157, 140, 255, 0.025);
    animation: drum-pulse 1.1s ease-in-out infinite;
}

.drum-stick {
    position: absolute;
    width: 150px;
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--violet));
    transform-origin: left center;
}

.stick-1 {
    transform: rotate(28deg);
    animation: hit-a 1.1s infinite;
}

.stick-2 {
    transform: rotate(152deg);
    animation: hit-b 1.1s 0.18s infinite;
}

.sound-wave {
    position: absolute;
    border: 1px solid rgba(157, 140, 255, 0.25);
    border-radius: 50%;
    animation: wave 2.4s infinite;
}

.wave-1 {
    width: 190px;
    height: 190px;
}

.wave-2 {
    width: 250px;
    height: 250px;
    animation-delay: .6s;
}

.wave-3 {
    width: 310px;
    height: 310px;
    animation-delay: 1.2s;
}

.quiz {
    display: grid;
    place-items: center;
    background: radial-gradient(circle, rgba(117, 242, 166, 0.09), transparent 45%), #07090e;
}

.quiz-screen {
    width: 130px;
    height: 150px;
    padding: 1rem;
    border: 1px solid rgba(117, 242, 166, 0.3);
    border-radius: 10px;
    background: rgba(117, 242, 166, 0.03);
    box-shadow: 0 25px 70px rgba(0, 0, 0, 0.35);
}

.quiz-screen span {
    display: grid;
    place-items: center;
    height: 75px;
    color: var(--green);
    font: 700 3rem 'Space Grotesk', sans-serif;
}

.quiz-screen i {
    display: block;
    width: 80%;
    height: 3px;
    margin: 7px auto;
    border-radius: 3px;
    background: rgba(117, 242, 166, 0.15);
}

.quiz-screen i:nth-child(3) {
    width: 60%;
}

.quiz-screen i:nth-child(4) {
    width: 70%;
}

.research {
    border-top: 1px solid var(--line);
}

.research-layout {
    display: grid;
    grid-template-columns: 1fr 0.8fr;
    align-items: center;
    gap: 5rem;
}

.research h2 {
    margin: 0;
    font: 600 clamp(2.5rem, 5vw, 5.5rem)/0.96 'Space Grotesk', sans-serif;
    letter-spacing: -0.06em;
}

.research h2 .small {
    display: block;
    margin-bottom: 0.8rem;
    color: var(--cyan);
    font: 0.65rem 'DM Mono', monospace;
    letter-spacing: 0.1em;
    text-transform: uppercase;
}

.research h2 em {
    color: var(--violet);
    font-style: normal;
}

.research-copy {
    max-width: 650px;
    margin: 2rem 0;
    color: var(--muted);
    line-height: 1.9;
}

.research-machine {
    position: relative;
    min-height: 500px;
    display: grid;
    place-items: center;
}

.machine-ring {
    position: absolute;
    border-radius: 50%;
    border: 1px solid rgba(103, 232, 249, 0.14);
}

.ring-1 {
    width: 340px;
    height: 340px;
    animation: spin 20s linear infinite;
}

.ring-2 {
    width: 470px;
    height: 470px;
    border-style: dashed;
    opacity: 0.5;
    animation: spin-reverse 28s linear infinite;
}

.machine-core {
    position: relative;
    z-index: 2;
    display: grid;
    place-items: center;
    width: 155px;
    height: 155px;
    border: 1px solid rgba(103, 232, 249, 0.3);
    border-radius: 50%;
    background: rgba(8, 11, 16, 0.88);
    box-shadow: 0 0 70px rgba(103, 232, 249, 0.1);
}

.core-eye {
    color: var(--cyan);
    font-size: 2rem;
    animation: blink 2.5s infinite;
}

.machine-core span {
    color: #687282;
    font: 0.52rem 'DM Mono', monospace;
    letter-spacing: 0.14em;
}

.machine-core strong {
    color: var(--violet);
    font: 500 0.75rem 'DM Mono', monospace;
}

.data-pill {
    position: absolute;
    z-index: 3;
    padding: 0.5rem 0.65rem;
    border: 1px solid var(--line);
    border-radius: 6px;
    background: rgba(7, 9, 13, 0.88);
    color: #778191;
    font: 0.55rem 'DM Mono', monospace;
}

.p1 {
    top: 8%;
    left: 12%;
}

.p2 {
    top: 25%;
    right: 3%;
}

.p3 {
    bottom: 20%;
    right: 0;
    color: var(--cyan);
}

.p4 {
    bottom: 7%;
    left: 14%;
}

.education {
    border-top: 1px solid var(--line);
}

.education-grid {
    display: grid;
    grid-template-columns: 0.75fr 1fr;
    gap: 5rem;
}

.education-list {
    display: grid;
    gap: 1rem;
}

.education-list article {
    padding: 1.5rem;
    border-left: 1px solid var(--line);
    transition: border-color 0.2s ease, padding-left 0.2s ease;
}

.education-list article:hover {
    padding-left: 2rem;
    border-color: var(--cyan);
}

.education-list article>span {
    color: var(--cyan);
    font: 0.6rem 'DM Mono', monospace;
}

.education-list h3 {
    margin: 0.65rem 0 0.25rem;
    font: 500 1.25rem 'Space Grotesk', sans-serif;
}

.education-list p {
    margin: 0 0 0.8rem;
    color: #8c95a3;
    font-size: 0.82rem;
}

.education-list small {
    color: #5e6775;
    line-height: 1.6;
}

.contact {
    padding-bottom: 5rem;
}

.contact-card {
    position: relative;
    min-height: 580px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    overflow: hidden;
    padding: 5rem;
    border: 1px solid var(--line);
    border-radius: 18px;
    background:
        radial-gradient(circle at 80% 20%, rgba(103, 232, 249, 0.12), transparent 30%),
        radial-gradient(circle at 15% 90%, rgba(157, 140, 255, 0.12), transparent 35%),
        #0b0f15;
}

.contact-orb {
    position: absolute;
    width: 360px;
    height: 360px;
    right: 8%;
    top: 10%;
    border: 1px solid rgba(103, 232, 249, 0.15);
    border-radius: 50%;
    box-shadow:
        0 0 0 40px rgba(103, 232, 249, 0.02),
        0 0 0 80px rgba(103, 232, 249, 0.015);
    animation: spin 30s linear infinite;
}

.contact-card .section-label,
.contact-card h2,
.contact-card p,
.contact-actions,
.contact-footer {
    position: relative;
    z-index: 2;
}

.contact-card h2 {
    max-width: 800px;
}

.contact-card>p {
    max-width: 560px;
    margin: 1.5rem 0 2rem;
    color: var(--muted);
    line-height: 1.8;
}

.contact-actions {
    display: flex;
    flex-wrap: wrap;
    gap: 0.8rem;
}

.contact-footer {
    display: flex;
    justify-content: space-between;
    margin-top: auto;
    padding-top: 5rem;
    color: #505968;
    font-size: 0.58rem;
}

.footer {
    position: relative;
    z-index: 2;
    max-width: 1400px;
    margin: 0 auto;
    padding: 0 3.5rem 2.5rem;
    display: flex;
    justify-content: space-between;
    color: #414957;
    font-size: 0.58rem;
}

@keyframes pulse {

    0%,
    100% {
        opacity: 1;
        box-shadow: 0 0 12px var(--green);
    }

    50% {
        opacity: 0.4;
        box-shadow: 0 0 4px var(--green);
    }
}

@keyframes spin {
    from {
        transform: rotate(0deg);
    }

    to {
        transform: rotate(360deg);
    }
}

@keyframes spin-reverse {
    from {
        transform: rotate(360deg);
    }

    to {
        transform: rotate(0deg);
    }
}

@keyframes float {

    0%,
    100% {
        transform: translateY(0);
    }

    50% {
        transform: translateY(-12px);
    }
}

@keyframes drive {
    from {
        transform: translateX(0);
    }

    to {
        transform: translateX(500px);
    }
}

@keyframes drive-v {
    from {
        transform: rotate(90deg) translateX(0);
    }

    to {
        transform: rotate(90deg) translateX(500px);
    }
}

@keyframes blink {

    0%,
    35%,
    100% {
        opacity: 1;
    }

    40%,
    50% {
        opacity: 0.35;
    }
}

@keyframes drum-pulse {

    0%,
    100% {
        transform: scale(1);
    }

    50% {
        transform: scale(0.93);
    }
}

@keyframes hit-a {

    0%,
    100% {
        transform: rotate(28deg);
    }

    45% {
        transform: rotate(8deg);
    }
}

@keyframes hit-b {

    0%,
    100% {
        transform: rotate(152deg);
    }

    45% {
        transform: rotate(172deg);
    }
}

@keyframes wave {
    0% {
        transform: scale(0.7);
        opacity: 0;
    }

    35% {
        opacity: 1;
    }

    100% {
        transform: scale(1.25);
        opacity: 0;
    }
}

@media (max-width: 1000px) {
    .nav-wrap {
        padding: 1rem 1.25rem;
    }

    .section {
        padding: 6rem 1.25rem;
    }

    .hero {
        grid-template-columns: 1fr;
        padding-top: 8rem;
    }

    .hero-visual {
        min-height: 500px;
    }

    .about-grid,
    .stack-wall,
    .project-grid,
    .research-layout,
    .education-grid {
        grid-template-columns: 1fr;
    }

    .project-1 {
        grid-row: auto;
    }

    .project-card,
    .project-1 {
        min-height: auto;
    }

    .section-heading.row-heading {
        grid-template-columns: 1fr;
    }

    .research-layout {
        gap: 2rem;
    }

    .education-grid {
        gap: 3rem;
    }

    .footer {
        padding: 0 1.25rem 2rem;
    }
}

@media (max-width: 720px) {
    .brand-text {
        font-size: 0.78rem;
    }

    .menu-btn {
        display: block;
    }

    .nav-links {
        position: absolute;
        top: calc(100% + 0.5rem);
        left: 1rem;
        right: 1rem;
        display: none;
        flex-direction: column;
        align-items: stretch;
        gap: 0;
        padding: 0.7rem;
        border: 1px solid var(--line);
        border-radius: 12px;
        background: rgba(10, 13, 18, 0.96);
        backdrop-filter: blur(18px);
    }

    .nav-links.open {
        display: flex;
    }

    .nav-links a {
        padding: 0.9rem;
    }

    .hero {
        min-height: auto;
        padding-top: 8rem;
    }

    .hero h1 {
        font-size: clamp(3rem, 15vw, 5rem);
    }

    .hero-description {
        font-size: 0.9rem;
    }

    .hero-meta {
        gap: 1.25rem;
        flex-wrap: wrap;
    }

    .hero-visual {
        min-height: 430px;
    }

    .code-card {
        width: 94%;
    }

    .orbit-a {
        width: 420px;
        height: 420px;
    }

    .orbit-b {
        width: 320px;
        height: 320px;
    }

    .orbit-c {
        width: 500px;
        height: 500px;
    }

    .floating-chip {
        display: none;
    }

    .about-grid {
        border: 0;
        background: transparent;
        gap: 0.7rem;
    }

    .manifesto-card {
        min-height: 0;
        border: 1px solid var(--line);
        border-radius: 10px;
    }

    .card-number {
        margin-bottom: 3rem;
    }

    .stack-wall {
        border: 0;
        background: transparent;
        gap: 0.7rem;
    }

    .skill-group {
        border: 1px solid var(--line);
        border-radius: 10px;
    }

    .timeline {
        margin-left: 0;
    }

    .timeline::before {
        left: 19px;
    }

    .timeline-item {
        grid-template-columns: 40px 1fr;
        gap: 0.9rem;
    }

    .timeline-marker {
        width: 40px;
        height: 40px;
    }

    .timeline-content {
        padding: 1rem;
    }

    .timeline-top {
        flex-direction: column;
        gap: 0.4rem;
    }

    .project-card {
        border-radius: 11px;
    }

    .project-1 .project-art {
        height: 280px;
    }

    .research-machine {
        min-height: 390px;
    }

    .ring-1 {
        width: 260px;
        height: 260px;
    }

    .ring-2 {
        width: 350px;
        height: 350px;
    }

    .data-pill {
        font-size: 0.48rem;
    }

    .contact-card {
        min-height: 520px;
        padding: 2rem 1.3rem;
    }

    .contact-orb {
        right: -100px;
        top: 20%;
    }

    .contact-footer,
    .footer {
        flex-direction: column;
        gap: 0.7rem;
    }
}

@media (prefers-reduced-motion: reduce) {

    *,
    *::before,
    *::after {
        scroll-behavior: auto !important;
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}
</style>
