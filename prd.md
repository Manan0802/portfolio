# MANAN KUMAR — 3D Portfolio PRD
## Product Requirements Document — Complete Build Spec for Cursor

---

## 🎯 PROJECT OVERVIEW

Build the **best AI developer portfolio on the internet** — a dark sci-fi, space-hacker aesthetic 3D portfolio that beats shubhambishnoi.in, akashrmalhotra.netlify.app, and moncy.dev in every dimension. References all three but surpasses them in identity, animations, 3D features, and client conversion.

**Owner:** Manan Kumar  
**Email:** 04manank@gmail.com  
**LinkedIn:** https://www.linkedin.com/in/manan-kumar-2229291b9/  
**GitHub:** https://github.com/Manan0802  
**LeetCode:** https://leetcode.com/u/04manank/  
**Deploy target:** Vercel (free tier)  
**Domain:** manankumar.dev (or similar)

---

## 🛠️ TECH STACK — LOCKED

```
Framework:        React 18 + Vite + TypeScript
3D Engine:        React Three Fiber (@react-three/fiber)
3D Helpers:       @react-three/drei + @react-three/postprocessing
Physics:          @react-three/rapier
3D Model:         character.glb (from MoncyDev repo) — place in /public/models/
Animation:        GSAP + @gsap/react + ScrollTrigger (FREE tier — no Club plugins)
Smooth Scroll:    Lenis
UI Motion:        Framer Motion
Styling:          Tailwind CSS + custom CSS modules
Icons:            react-icons
Marquee:          react-fast-marquee
Email:            EmailJS (free, no backend needed)
Tilt effect:      vanilla-tilt.js
Cursor:           Custom CSS + JS
Analytics:        @vercel/analytics
Deploy:           Vercel
```

---

## 🎨 DESIGN SYSTEM

```
Background:       #000000 (pure black — no gradients on bg)
Primary accent:   #3B82F6 (electric blue)
Secondary accent: #8B5CF6 (purple)
Tertiary:         #06B6D4 (cyan — used sparingly)
Text primary:     #FFFFFF
Text secondary:   #9CA3AF
Glow effects:     Electric blue + purple — on hover, borders, active states
Glassmorphism:    background: rgba(255,255,255,0.03), backdrop-filter: blur(10px), border: 1px solid rgba(255,255,255,0.08)
Font:             'Space Grotesk' (headings) + 'Inter' (body) — Google Fonts
Hero name font:   'Bebas Neue' or 'Monument Extended' — weight 900, massive
```

---

## 📁 PROJECT STRUCTURE

```
portfolio/
├── public/
│   ├── models/
│   │   └── character.glb          ← 3D character model (PLACE HERE)
│   ├── draco/                      ← draco decoder files (copy from drei)
│   ├── images/
│   │   ├── projects/
│   │   │   ├── investmate.png      ← project screenshots (shots.so mockups)
│   │   │   ├── crop-yield.png
│   │   │   └── travel-app.png
│   │   └── logo/
│   ├── resume/
│   │   └── Manan_Kumar_Resume.pdf  ← resume for download button
│   └── favicon.ico
├── src/
│   ├── components/
│   │   ├── Loader/
│   │   │   ├── Loader.tsx          ← intro/loading screen
│   │   │   └── Loader.css
│   │   ├── Cursor/
│   │   │   ├── CustomCursor.tsx    ← custom glow cursor + trail
│   │   │   └── Cursor.css
│   │   ├── Navbar/
│   │   │   └── Navbar.tsx
│   │   ├── Character/
│   │   │   ├── CharacterScene.tsx  ← R3F Canvas wrapper
│   │   │   ├── CharacterModel.tsx  ← GLB loader + animations
│   │   │   └── Particles.tsx       ← galaxy particle field
│   │   ├── Hero/
│   │   │   └── Hero.tsx
│   │   ├── About/
│   │   │   ├── About.tsx
│   │   │   ├── SkillsGlobe.tsx     ← 3D rotating skills sphere
│   │   │   └── Story.tsx
│   │   ├── Experience/
│   │   │   ├── Experience.tsx
│   │   │   └── Timeline.tsx
│   │   ├── Projects/
│   │   │   ├── Projects.tsx
│   │   │   ├── ProjectCard.tsx     ← tilt + glassmorphism card
│   │   │   ├── ProjectModal.tsx    ← full case study modal
│   │   │   └── FeaturedProject.tsx ← 3D MacBook hero project
│   │   ├── Services/
│   │   │   └── Services.tsx        ← freelance services section
│   │   ├── Contact/
│   │   │   └── Contact.tsx
│   │   └── shared/
│   │       ├── GlowButton.tsx
│   │       └── SectionTitle.tsx
│   ├── data/
│   │   ├── projects.ts             ← all project data
│   │   ├── experience.ts           ← work experience data
│   │   ├── skills.ts               ← skills list
│   │   └── testimonials.ts         ← testimonials
│   ├── hooks/
│   │   ├── useScrollAnimation.ts
│   │   └── useCursor.ts
│   ├── context/
│   │   └── LoadingContext.tsx
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
├── package.json
└── vite.config.ts
```

---

## 🚀 SECTION 0 — LOADING / INTRO SCREEN

**File:** `src/components/Loader/Loader.tsx`

This is the FIRST thing the visitor sees — cinematic entry, sets the entire vibe.

### Behavior:
1. Full screen pure black takeover
2. Center: glitch text animation cycling through:
   - `"INITIALIZING SYSTEMS..."` 
   - `"LOADING AI MODULES..."` 
   - `"ESTABLISHING CONNECTION..."` 
   - `"WELCOME, HUMAN"`
3. Bottom: electric blue progress bar animating 0% → 100%
4. Progress bar fills as actual assets load (GLB model + images)
5. At 100%: particles EXPLODE outward from center
6. Screen fades/wipes to reveal Hero section
7. Character already standing in hero with `introAnimation` playing

### Visual Details:
- Glitch effect: chromatic aberration on text (red/blue shadow offset), random character flicker
- Progress bar: glowing neon blue, `box-shadow: 0 0 10px #3B82F6, 0 0 20px #3B82F6`
- `"MANAN KUMAR"` appears letter by letter with glitch during loading
- Small scanline overlay on entire screen (CSS `repeating-linear-gradient`)
- Loading percentage number in monospace font top-right corner

### Code pattern:
```tsx
// Use R3F useProgress for actual asset loading progress
import { useProgress } from '@react-three/drei'
const { progress } = useProgress() // 0-100 as GLB loads
```

---

## 🌌 SECTION 1 — HERO

**File:** `src/components/Hero/Hero.tsx` + `src/components/Character/`

### Layout:
- Full viewport height (100vh)
- Pure black background
- `"MANAN KUMAR"` — massive text, `font-size: clamp(80px, 15vw, 200px)`, weight 900, white, positioned BEHIND the 3D character (z-index trick)
- Character GLB centered, overlapping/clipping in front of the name text
- Tagline below character (typewriter rotating effect)
- Two CTA buttons bottom: `"View My Work"` + `"Download Resume"`
- Scroll indicator bouncing arrow at very bottom

### 3D Character Scene (`CharacterScene.tsx`):
```tsx
<Canvas
  camera={{ position: [0, 0, 5], fov: 45 }}
  style={{ position: 'absolute', top: 0, left: 0, width: '100%', height: '100%' }}
  gl={{ antialias: true, alpha: true }}  // alpha: true for transparent bg
>
  <Suspense fallback={null}>
    <CharacterModel />
    <Particles count={15000} />
    <spotLight position={[0, 5, 3]} intensity={100} angle={0.3} castShadow />
    <ambientLight intensity={0.05} />
    <EffectComposer>
      <Bloom intensity={0.3} luminanceThreshold={0.8} />
    </EffectComposer>
  </Suspense>
</Canvas>
```

### Character Animations (`CharacterModel.tsx`):
```tsx
// On load: play introAnimation once
// Always: loop Blink animation
// On scroll: rotate head Y-axis based on scroll offset
// On mouse move: subtle body tilt (rotation.x, rotation.z by ±0.05)
// On hover (if mouse near character): play browup animation

const { scene, animations } = useGLTF('/models/character.glb')
const { actions } = useAnimations(animations, scene)

// Available animations in this GLB:
// typing, browup, angry, introAnimation, Blink, key1, key2, key3, key4, key5

useEffect(() => {
  actions['introAnimation']?.play()  // play once on load
  actions['Blink']?.play()           // loop forever
}, [actions])

useFrame(({ mouse }) => {
  // Mouse parallax on character body
  meshRef.current.rotation.y = THREE.MathUtils.lerp(
    meshRef.current.rotation.y, mouse.x * 0.3, 0.05
  )
  meshRef.current.rotation.x = THREE.MathUtils.lerp(
    meshRef.current.rotation.x, -mouse.y * 0.1, 0.05
  )
  // Scroll-based head rotation (head bone specifically)
  headBone.rotation.y = scroll.offset * Math.PI * 2
})
```

### Particle Galaxy (`Particles.tsx`):
```tsx
// 15,000 points in a galaxy/sphere formation
// Slowly rotating on Y axis
// Mouse move = entire galaxy subtly shifts (parallax)
// Color: mix of blue (#3B82F6) and purple (#8B5CF6) dots
// ShaderMaterial for glowing round points
```

### Typewriter Tagline:
```
Cycles through (with typing + deleting cursor effect):
1. "Building AI Agents"
2. "LLM & Agentic Workflows"  
3. "Full Stack Developer"
4. "ML Engineer @ IndiaMART"
```

### Custom Cursor (`CustomCursor.tsx`):
- Small dot (4px) — follows mouse exactly
- Larger ring (40px) — follows with lerp delay (magnetic lag)
- On hover over clickable elements: ring expands to 80px, text inside changes to "VIEW" / "OPEN" / "CLICK"
- On hover over character: ring turns purple, text "INTERACT"
- CSS only for movement, JS for state changes

---

## 👤 SECTION 2 — ABOUT + SKILLS + STORY

**File:** `src/components/About/About.tsx`

### About Me Sub-section:
- Two column layout: left = text, right = character (smaller, typing animation active)
- When scrolled into view: character switches from idle → `typing` animation
- `key1` through `key5` animations fire in sequence as keys animate
- Text reveals with GSAP SplitText (word by word, staggered)

**Bio text:**
```
Software Engineering student at Delhi Technological University (DTU), 
specializing in Applied AI, LLM Agentic Workflows, and Scalable Backend Systems.

Currently at IndiaMART InterMESH, architecting AI pipelines that process 
10 lakh+ records and deploying agentic systems at production scale.

Passionate about automating complex workflows, building intelligent systems, 
and reducing operational costs through innovative engineering.
```

### Animated Stat Counters (scroll into view triggers count-up):
```
400+    DSA Problems Solved
1650+   LeetCode Rating  
1M+     Records Processed (IndiaMART)
3+      AI Projects Deployed
```

### 3D Skills Globe (`SkillsGlobe.tsx`):
- Sphere of orbiting tech skill tags using `@react-three/drei` TagCloud or custom sphere distribution
- AI/ML skills = purple (#8B5CF6) glow
- Frontend skills = blue (#3B82F6) glow  
- Backend/DB skills = cyan (#06B6D4) glow
- Hover on skill = zoom in + name tooltip + extra glow burst
- Auto-rotating on Y axis, mouse drag to spin

**Skills data:**
```
AI/ML (purple): Python, PyTorch, LLMs, AI Agents, Prompt Engineering, 
                Deep Learning, CNNs, RNNs, n8n, Gemini API

Frontend (blue): React.js, Next.js, HTML5, CSS3, Tailwind CSS, Chart.js

Backend (cyan): Node.js, Express.js, MongoDB, MySQL, Redis, Firebase, 
                REST APIs, JWT

Languages/Tools (white): C++, JavaScript, Git, Power BI, VS Code
```

### My Story Sub-section:
- Dark background, centered text
- GSAP ScrollTrigger: each sentence/line reveals as you scroll
- Thin glowing vertical line on left as timeline indicator
- Soft paragraph:
```
"It started with a curiosity about how machines could think.

From cracking JEE with AIR 15,233 to joining DTU's Software Engineering program,
I always chased the intersection of intelligence and engineering.

At IndiaMART, I built AI systems that normalize data for 17,000+ product categories —
real pipelines, real scale, real impact.

Now I'm obsessed with Agentic AI — systems that don't just respond, but think, 
plan, and act autonomously.

This portfolio is not just my work. It's proof of what's possible."
```

---

## 💼 SECTION 3 — EXPERIENCE

**File:** `src/components/Experience/Experience.tsx`

### Timeline Layout:
- Vertical glowing line down center (GSAP drawSVG animates as you scroll)
- Each node: glowing dot + ring pulse animation on entry
- Cards alternate left/right of center line
- Cards slide in from their respective side (GSAP x: ±100 → 0)

### Experience Cards:

**Card 1 — IndiaMART:**
```
Company:  IndiaMART InterMESH Ltd
Role:     Associate SWE Intern — AI Engineer
Duration: Jan 2026 – Present  |  📍 Noida, India
Tags:     LLM Agents · Redis · Prompt Engineering · Python · AI Pipelines

Highlights:
• AI normalization agent across 17,000+ MCATs — 98% data consistency
• LLM pipeline extracting specs from 1000s of PDFs — 3 hierarchical tiers
• Redis workflow processing 10 Lakh+ (1M+) MCAT IDs at scale
• Transitioned manual cataloging → automated AI-enabled solutions
```

**Education Cards (below experience):**
```
DTU — B.Tech Software Engineering — 2022-2026 — CGPA: 7.75
Class XII — Sachdeva Public School — 88.8%
JEE Mains — AIR 15,233 | Math: 98 | Physics: 94
```

### Certifications Marquee (react-fast-marquee):
```
Infinite scrolling strip:
DeepLearning.AI — Deep Learning Specialization  ·  
Stanford & DeepLearning.AI — Machine Learning  ·  
Google — Prompting Essentials  ·  
Meta — Full Stack Developer Professional  ·  [repeats]
```

### Achievement Badges:
```
🏆 Top 10% Globally — LeetCode (400+ problems)
🥈 Silver Medal — Kabaddi (DTU Inter-Department)
🎯 Contest Rating 1650+ — Top 20% Globally
```

---

## 🚀 SECTION 4 — PROJECTS

**File:** `src/components/Projects/Projects.tsx`

### Filter Tabs (top):
```
[ All ] [ AI/LLM ] [ ML/Data ] [ Full Stack ]
```
- Pill style tabs with glowing active state
- GSAP layout animation on filter — cards smoothly reposition (no jump)

### Featured Project (top, full-width):

**InvestMate — AI-Powered Portfolio Tracker**
```
Layout: Full width card, dark glass
Left side: Text + tech stack + links
Right side: 3D MacBook model (drei <Float> component, gentle bob)
         MacBook screen shows: actual InvestMate dashboard screenshot/GIF

On scroll into view: MacBook lid slowly opens (rotation animation)
Links: GitHub button + Live App button (glow on hover)
Tag: AI/LLM

Key metrics shown as badges:
  100+ Users  |  <70ms Updates  |  1000+ Assets  |  80% Better Decisions
```

### Project Grid (2-3 columns):

**Card 1 — Crop Yield Prediction:**
```
Image:   Streamlit app screenshot in browser mockup (shots.so)
Title:   Crop Yield Prediction & Fertilizer Recommender
Desc:    ML regression model (R²=0.91) predicting crop yield + explainable fertilizer advisor
Tags:    ML/Data
Stack:   Python · Streamlit · PyTorch · Pandas · NumPy
Links:   GitHub · Colab
Hover:   Card tilts toward cursor (vanilla-tilt), image zooms, border glows
```

**Card 2 — Tour & Travel App:**
```
Image:   React UI screenshot in browser mockup
Title:   Tour & Travel App UI
Desc:    Responsive React UI with 30+ destinations, 90%+ Lighthouse score, mobile-first
Tags:    Full Stack
Stack:   React.js · Tailwind CSS · HTML5
Links:   GitHub
Hover:   Tilt + zoom + glow
```

### Project Modal (on card click):
```tsx
// Smooth expand animation — card scales up to fill screen
// Backdrop: blur + dark overlay on rest of page
// Modal content:
//   - Large project image/GIF at top
//   - Title + one-liner
//   - Problem statement
//   - Solution approach
//   - Tech stack tags
//   - Key metrics/results
//   - Live Demo button + GitHub button
//   - Close button (X top right)
// Close: click outside or X button → reverse animation
```

---

## 🤝 SECTION 5 — FREELANCE SERVICES

**File:** `src/components/Services/Services.tsx`

### Availability Badge (top of section):
```tsx
<div className="availability-badge">
  <span className="pulse-dot" /> {/* pulsing green dot */}
  Available for Internships & Freelance Projects
</div>
// CSS: green dot with box-shadow pulse animation
```

### Service Cards (3D flip on hover):

**Card Front:** Icon + Service name + one liner  
**Card Back (flip):** What's included (bullet list)

```
Card 1: 🤖 Agentic AI Systems
  Back: LLM pipeline design · Multi-agent workflows · 
        n8n automations · Cost optimization · Production deployment

Card 2: 🧠 ML Model Integration
  Back: Custom model training · API integration · 
        Real-time inference · PyTorch/Scikit-learn · Performance tuning

Card 3: 💻 Full Stack Web Apps  
  Back: React/Next.js frontend · Node.js backend · 
        MongoDB/MySQL · REST APIs · Auth systems · Deployment

Card 4: 📊 AI-Powered Dashboards
  Back: Real-time data viz · Chart.js/Recharts · 
        Live APIs integration · KPI tracking · Custom analytics
```

### My Process (animated connector):
```
Step 1: Understand   →   Step 2: Plan   →   Step 3: Build   →   Step 4: Deliver
[dotted animated line connects steps as they scroll into view]
```

### Testimonials (2 cards, glassmorphism):

```
Testimonial 1:
"Manan delivered an AI data pipeline that processed our entire catalog — 
17,000+ categories — with 98% accuracy. The system he built would have 
taken our team months manually. Exceptional engineering thinking."
— Senior Category Manager, IndiaMART InterMESH

Testimonial 2:
"Working with Manan on InvestMate was seamless. He integrated Gemini AI 
recommendations that genuinely improved how we understand our portfolios. 
Rare to find someone who bridges AI and product so effortlessly."
— Early User, InvestMate Platform
```

---

## 📬 SECTION 6 — CONTACT

**File:** `src/components/Contact/Contact.tsx`

### Layout:
- Starfield particle background (reuse from Hero — same component, different density)
- Left: contact info + social links
- Right: contact form

### Contact Form (EmailJS):
```
Fields: Name · Email · Message · [Dropdown: "Looking to Hire" / "Freelance Project" / "Collaboration"]
Submit button: "Send Message 🚀" → on click: rocket emoji animation fires across screen → success state
Form validation: inline error states with red glow
```

### Social Links (with hover animations):
```tsx
// Each icon: hover = bounce up + glow burst
GitHub    → https://github.com/Manan0802
LinkedIn  → https://www.linkedin.com/in/manan-kumar-2229291b9/
LeetCode  → https://leetcode.com/u/04manank/
Email     → 04manank@gmail.com (mailto link)
```

### Dual CTAs:
```
[  Hire Me — Full Time  ]    [  Let's Build Something  ]
     (jobs/internship)              (freelance)
```

### Footer:
```
© 2025 Manan Kumar · Built with React + Three.js
[GitHub] [LinkedIn] [LeetCode] [Email]
"Designed & Developed by Manan Kumar"
```

---

## 🌐 GLOBAL FEATURES

### Navbar:
```
Left: "MK" logo (glitch animation on hover)
Right: Hero · About · Experience · Projects · Services · Contact · [Download Resume button]
Mobile: hamburger → full screen menu with staggered link entry
Scrolled state: backdrop-blur + border-bottom glow
```

### Lenis Smooth Scroll:
```tsx
// In main.tsx or App.tsx
import Lenis from 'lenis'
const lenis = new Lenis({ duration: 1.2, easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)) })
// Connect to GSAP ScrollTrigger
gsap.ticker.add((time) => { lenis.raf(time * 1000) })
```

### Mobile Responsive Rules:
```
Desktop (>1024px): Full 3D, all effects, character center
Tablet (768-1024px): Reduce particle count to 5000, character smaller
Mobile (<768px): 3D canvas hidden or ultra-simplified (static image fallback),
                 content sections fully responsive, no tilt effects
```

### Performance:
```
- useProgress from drei for loading management
- Draco compression on GLB (run: npx gltf-transform optimize character.glb character-opt.glb --draco)
- R3F frameloop="demand" — only renders when something changes
- React.lazy + Suspense for section components
- Image: WebP format, lazy loading
- Target: 60fps desktop, 30fps mobile
```

### SEO + Meta:
```html
<!-- In index.html -->
<title>Manan Kumar — AI Engineer & Full Stack Developer</title>
<meta name="description" content="Building AI Agents, LLM Workflows & Full Stack Apps. Software Engineering student at DTU, AI Intern at IndiaMART." />
<meta property="og:title" content="Manan Kumar — Portfolio" />
<meta property="og:description" content="AI Engineer building agentic systems at scale." />
<meta property="og:image" content="/images/og-preview.png" /> <!-- nice screenshot of your site -->
```

---

## 📦 PACKAGE.JSON DEPENDENCIES

```json
{
  "dependencies": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "three": "^0.169.0",
    "@react-three/fiber": "^8.17.10",
    "@react-three/drei": "^9.115.0",
    "@react-three/postprocessing": "^2.16.3",
    "@react-three/rapier": "^1.5.0",
    "gsap": "^3.12.5",
    "@gsap/react": "^2.1.1",
    "lenis": "^1.1.14",
    "framer-motion": "^11.11.17",
    "vanilla-tilt": "^1.8.1",
    "react-icons": "^5.3.0",
    "react-fast-marquee": "^1.6.5",
    "@emailjs/browser": "^4.4.1",
    "@vercel/analytics": "^1.3.2",
    "tailwindcss": "^3.4.14"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.3.3",
    "typescript": "^5.6.3",
    "vite": "^5.4.11"
  }
}
```

---

## ⚡ GETTING STARTED — CURSOR BUILD COMMANDS

```bash
# 1. Create project
npm create vite@latest manan-portfolio -- --template react-ts
cd manan-portfolio

# 2. Install all dependencies
npm install three @react-three/fiber @react-three/drei @react-three/postprocessing @react-three/rapier gsap @gsap/react lenis framer-motion vanilla-tilt react-icons react-fast-marquee @emailjs/browser @vercel/analytics

# 3. Install Tailwind
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# 4. Place character.glb
# Copy character.glb → public/models/character.glb

# 5. Start dev server
npm run dev
```

---

## 🎬 BUILD ORDER (Section by Section in Cursor)

```
Phase 1: Foundation
  □ Project setup + all installs
  □ Tailwind config + global CSS (design system variables)
  □ Lenis smooth scroll setup
  □ Custom cursor component
  □ Navbar component

Phase 2: Loader + Hero (THE MOST IMPORTANT — do this first)
  □ Loading screen with progress bar + glitch text
  □ Character GLB loader (CharacterModel.tsx)
  □ Particle galaxy (Particles.tsx)  
  □ Hero layout — name behind character
  □ Typewriter tagline
  □ Scroll rotation on character

Phase 3: About + Skills
  □ About section layout + bio text
  □ Stat counters animation
  □ 3D Skills Globe
  □ My Story scroll reveal

Phase 4: Experience
  □ GSAP timeline draw animation
  □ Alternating experience cards
  □ Certifications marquee

Phase 5: Projects
  □ Filter tabs
  □ Featured project (MacBook 3D)
  □ Project grid cards (tilt + glassmorphism)
  □ Project modal (case study)

Phase 6: Services + Contact
  □ Service flip cards
  □ Availability badge
  □ Testimonials
  □ Contact form (EmailJS)
  □ Footer

Phase 7: Polish
  □ Mobile responsive pass
  □ Performance optimization (Draco, frameloop demand)
  □ SEO meta tags
  □ Loading screen polish
  □ Final animation tuning

Phase 8: Deploy
  □ npm run build
  □ Vercel deploy
  □ Custom domain setup
  □ LinkedIn + GitHub bio update with live link
```

---

## 🖼️ PROJECT IMAGES — HOW TO GET THEM

```
For each project:
1. Open the project locally / live site
2. Take full-page screenshot (browser DevTools → Ctrl+Shift+P → "Capture full size screenshot")
3. Go to shots.so → Upload screenshot → Select "Browser" frame style → Dark theme → Export PNG
4. Place in /public/images/projects/ folder
5. For hover GIF: record 5-10 sec demo with Loom or OBS → convert to GIF → place alongside PNG

Files needed:
  public/images/projects/investmate.png      (+ investmate.gif for hover)
  public/images/projects/crop-yield.png      (+ crop-yield.gif)
  public/images/projects/travel-app.png      (+ travel-app.gif)
```

---

## ✅ FINAL CHECKLIST BEFORE LAUNCH

```
□ character.glb in public/models/
□ Manan_Kumar_Resume.pdf in public/resume/
□ All project images in public/images/projects/
□ EmailJS account setup (service ID, template ID, public key in .env)
□ All social links working
□ Mobile responsive tested (iPhone + Android)
□ 60fps desktop performance verified
□ OG preview image created (1200x630px screenshot of site)
□ Vercel deploy successful
□ Custom domain connected (optional)
□ LinkedIn bio updated with live URL
□ GitHub pinned repos match portfolio projects
```

---

*PRD Version 1.0 — Manan Kumar Portfolio — Prepared for Cursor AI Build*
*Reference sites: shubhambishnoi.in · akashrmalhotra.netlify.app · moncy.dev*
*Model: character.glb (MoncyDev repo — MIT License)*