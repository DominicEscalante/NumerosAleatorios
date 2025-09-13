<template>
  <div class="home-container">
    <Navbar />
    <Background />

    <div class="floating-elements">
      <div class="float-element float-1"></div>
      <div class="float-element float-2"></div>
      <div class="float-element float-3"></div>
      <div class="float-element float-4"></div>
    </div>

    <div class="scroll-container" ref="scrollContainer" @wheel="handleWheel">
      <section class="hero-section" ref="heroSection">
        <div class="hero-content">
          <h1 class="hero-title animated-title">Rand#</h1>
          <p class="hero-subtitle animated-subtitle">
            Generador de números aleatorios con distintos métodos
          </p>
          <div class="scroll-indicator animated-indicator" @click="scrollToCards">
            <span class="pulse-ring"></span>
            <span class="arrow">↓</span>
          </div>
        </div>
        <div class="animated-waves">
          <div class="wave wave1"></div>
          <div class="wave wave2"></div>
          <div class="wave wave3"></div>
        </div>
      </section>

      <section class="cards-section" ref="cardsSection">
        <div class="section-title animated-section-title">
          <h2>Métodos de Generación</h2>
          <div class="title-underline"></div>
        </div>

        <div class="cards-container">
          <div class="algorithm-card card-1" @mouseenter="onCardHover" @mouseleave="onCardLeave">
            <div class="card-glow"></div>
            <div class="card-content">
              <div class="card-icon">🔢</div>
              <h2>Algoritmo Lineal Congruencial</h2>
              <p>
                Un método clásico que genera números pseudoaleatorios usando la fórmula
                <br /><code>X(n+1) = (a * X(n) + c) mod m</code>. <br />Simple pero efectivo para
                muchas aplicaciones.
              </p>
              <button class="btn animated-btn" @click="probar('lineal')">
                <span class="btn-text">Probar Algoritmo</span>
                <span class="btn-icon">→</span>
              </button>
            </div>
          </div>

          <div class="algorithm-card card-2" @mouseenter="onCardHover" @mouseleave="onCardLeave">
            <div class="card-glow"></div>
            <div class="card-content">
              <div class="card-icon">⏹️</div>
              <h2>Algoritmo Congruencial Multiplicativo</h2>
              <p>
                Es una variante del generador lineal congruencial, pero sin el término de
                incremento. Se define como:
                <br /><code>Xₙ₊₁ = (aXₙ) mod m</code>. <br />Donde: <br />- <strong>a</strong>:
                multiplicador <br />- <strong>m</strong>: módulo <br />- <strong>X₀</strong>:
                semilla inicial
              </p>
              <button class="btn animated-btn" @click="probar('multiplicativo')">
                <span class="btn-text">Probar Algoritmo</span>
                <span class="btn-icon">→</span>
              </button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import Background from '../components/Background.vue'
import Navbar from '@/components/Navbar.vue'

export default {
  name: 'HomeView',
  components: {
    Background,
    Navbar,
  },
  data() {
    return {
      isScrolling: false,
      currentSection: 0,
    }
  },
  mounted() {
    this.activateEntryAnimations()
    this.setupScrollObserver()
  },
  methods: {
    probar(tipo) {
      this.animateExit(() => {
        if (tipo === 'lineal') {
          this.$router.push('/lineal')
        } else if (tipo === 'multiplicativo') {
          this.$router.push('/multiplicativo')
        }
      })
    },

    scrollToCards() {
      this.scrollToSection(1)
    },

    scrollToSection(sectionIndex) {
      const sections = [this.$refs.heroSection, this.$refs.cardsSection]
      if (sections[sectionIndex]) {
        this.currentSection = sectionIndex
        sections[sectionIndex].scrollIntoView({
          behavior: 'smooth',
          block: 'start',
        })
      }
    },

    handleWheel(event) {
      if (this.isScrolling) return

      event.preventDefault()
      this.isScrolling = true

      const delta = event.deltaY > 0 ? 1 : -1
      let targetSection = this.currentSection + delta

      if (targetSection < 0) targetSection = 0
      if (targetSection > 1) targetSection = 1

      if (targetSection !== this.currentSection) {
        this.scrollToSection(targetSection)
      }

      setTimeout(() => {
        this.isScrolling = false
      }, 800)
    },

    activateEntryAnimations() {
      setTimeout(() => {
        document.querySelector('.animated-title')?.classList.add('animate-in')
      }, 300)

      setTimeout(() => {
        document.querySelector('.animated-subtitle')?.classList.add('animate-in')
      }, 600)

      setTimeout(() => {
        document.querySelector('.animated-indicator')?.classList.add('animate-in')
      }, 900)
    },

    setupScrollObserver() {
      const observer = new IntersectionObserver(
        (entries) => {
          entries.forEach((entry) => {
            if (entry.isIntersecting) {
              entry.target.classList.add('animate-in')
              if (entry.target.classList.contains('cards-container')) {
                const cards = entry.target.querySelectorAll('.algorithm-card')
                cards.forEach((card, index) => {
                  setTimeout(() => {
                    card.classList.add('animate-in')
                  }, index * 200)
                })
              }
            }
          })
        },
        { threshold: 0.1 },
      )

      document.querySelectorAll('.animated-section-title, .cards-container').forEach((el) => {
        observer.observe(el)
      })
    },

    onCardHover(event) {
      const card = event.currentTarget
      card.style.transform = 'translateY(-12px) scale(1.03)'
    },

    onCardLeave(event) {
      const card = event.currentTarget
      card.style.transform = ''
    },

    animateExit(callback) {
      const container = document.querySelector('.home-container')
      container.style.transition = 'all 0.5s ease'
      container.style.transform = 'scale(0.95)'
      container.style.opacity = '0.7'

      setTimeout(() => {
        callback()
      }, 500)
    },
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;700;900&display=swap');

.home-container {
  margin: 0;
  padding: 0;
  width: 100%;
  min-height: 100vh;
  position: relative;
  overflow: hidden;
}

.floating-elements {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1;
}

.float-element {
  position: absolute;
  width: 4px;
  height: 4px;
  background: linear-gradient(45deg, #00d4ff, #4facfe);
  border-radius: 50%;
  opacity: 0.6;
}

.float-1 {
  top: 20%;
  left: 10%;
  animation: float 6s ease-in-out infinite;
}

.float-2 {
  top: 60%;
  right: 15%;
  animation: float 8s ease-in-out infinite reverse;
}

.float-3 {
  top: 40%;
  left: 80%;
  animation: float 7s ease-in-out infinite;
}

.float-4 {
  bottom: 30%;
  left: 20%;
  animation: float 5s ease-in-out infinite reverse;
}

@keyframes float {
  0%,
  100% {
    transform: translateY(0) rotate(0deg);
  }
  25% {
    transform: translateY(-20px) rotate(90deg);
  }
  50% {
    transform: translateY(-40px) rotate(180deg);
  }
  75% {
    transform: translateY(-20px) rotate(270deg);
  }
}

.scroll-container {
  height: 100vh;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
  scroll-behavior: smooth;
  scroll-padding: 0;
  scroll-snap-stop: normal;
}

.hero-section {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  scroll-snap-align: start;
  position: relative;
  z-index: 10;
  padding-bottom: 2rem;
  overflow: hidden;
}

.hero-content {
  text-align: center;
  color: white;
  padding: 2rem;
  transform: translateY(-3rem);
  position: relative;
  z-index: 20;
}

.animated-title {
  font-size: 6rem;
  font-weight: 900;
  font-family: 'Orbitron', sans-serif;
  background: linear-gradient(90deg, #00f2fe, #4facfe, #00c9ff, #00f2fe);
  background-size: 300% 300%;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-shadow: 0 0 30px rgba(0, 200, 255, 0.8);
  margin-bottom: 1rem;
  letter-spacing: 3px;
  opacity: 0;
  transform: translateY(50px);
  transition: all 1s ease;
  animation: gradientFlow 4s ease-in-out infinite;
}

.animated-title.animate-in {
  opacity: 1;
  transform: translateY(0);
}

@keyframes gradientFlow {
  0%,
  100% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
}

.animated-subtitle {
  font-size: 1.5rem;
  color: #cceeff;
  font-family: 'Orbitron', sans-serif;
  font-weight: 400;
  margin-bottom: 3rem;
  opacity: 0;
  transform: translateY(30px);
  transition: all 1s ease 0.3s;
  animation: textGlow 3s ease-in-out infinite;
}

.animated-subtitle.animate-in {
  opacity: 0.9;
  transform: translateY(0);
}

@keyframes textGlow {
  0%,
  100% {
    text-shadow: 0 0 10px rgba(204, 238, 255, 0.5);
  }
  50% {
    text-shadow:
      0 0 20px rgba(204, 238, 255, 0.8),
      0 0 30px rgba(0, 212, 255, 0.3);
  }
}

.animated-indicator {
  position: relative;
  display: inline-block;
  opacity: 0;
  transform: translateY(20px);
  transition: all 1s ease 0.6s;
  cursor: pointer;
}

.animated-indicator.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.pulse-ring {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 60px;
  height: 60px;
  border: 2px solid #00d4ff;
  border-radius: 50%;
  animation: pulse 2s ease-out infinite;
}

.arrow {
  position: relative;
  font-size: 2rem;
  color: #00d4ff;
  display: block;
  animation: bounce 2s ease-in-out infinite;
  transition: all 0.3s ease;
  z-index: 10;
}

.animated-indicator:hover .arrow {
  transform: scale(1.3);
  color: #ffffff;
}

@keyframes pulse {
  0% {
    opacity: 1;
    transform: translate(-50%, -50%) scale(0.8);
  }
  100% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(2);
  }
}

@keyframes bounce {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(15px);
  }
}

.animated-waves {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 150px;
  z-index: 5;
}

.wave {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 200%;
  height: 100px;
  background: linear-gradient(45deg, rgba(0, 212, 255, 0.1), rgba(79, 172, 254, 0.1));
  border-radius: 50%;
}

.wave1 {
  animation: wave 8s ease-in-out infinite;
}

.wave2 {
  animation: wave 10s ease-in-out infinite reverse;
  opacity: 0.5;
}

.wave3 {
  animation: wave 12s ease-in-out infinite;
  opacity: 0.3;
}

@keyframes wave {
  0%,
  100% {
    transform: translateX(-50%) rotate(0deg);
  }
  50% {
    transform: translateX(-50%) rotate(180deg);
  }
}

.cards-section {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  scroll-snap-align: start;
  padding: 2rem 1rem;
  position: relative;
  z-index: 10;
  margin-top: -4rem;
}

.section-title {
  text-align: center;
  margin-bottom: 3rem;
  opacity: 0;
  transform: translateY(30px);
  transition: all 1s ease;
}

.section-title.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.section-title h2 {
  font-size: 2.5rem;
  font-weight: 700;
  font-family: 'Orbitron', sans-serif;
  color: white;
  margin-bottom: 1rem;
  background: linear-gradient(90deg, #00f2fe, #4facfe);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.title-underline {
  width: 100px;
  height: 3px;
  background: linear-gradient(90deg, #00f2fe, #4facfe);
  margin: 0 auto;
  border-radius: 2px;
  animation: expand 2s ease-out;
}

@keyframes expand {
  0% {
    width: 0;
  }
  100% {
    width: 100px;
  }
}

.cards-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 2.5rem;
  max-width: 1200px;
  width: 100%;
}

.algorithm-card {
  position: relative;
  background: radial-gradient(circle at top, rgba(40, 60, 100, 0.9), rgba(10, 10, 30, 0.95));
  border: 2px solid rgba(120, 200, 255, 0.6);
  backdrop-filter: blur(15px);
  border-radius: 1.5rem;
  padding: 0;
  box-shadow:
    0 0 25px rgba(0, 150, 255, 0.4),
    0 0 50px rgba(0, 50, 150, 0.3);
  text-align: center;
  color: #e0f7ff;
  transition: all 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  font-family: 'Orbitron', sans-serif;
  opacity: 0;
  transform: translateY(50px);
  overflow: hidden;

  backface-visibility: hidden;
  transform-style: preserve-3d;
  will-change: transform, box-shadow;
}

.algorithm-card.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.algorithm-card:hover {
  transform: translateY(-12px) scale(1.03);
  box-shadow:
    0 20px 40px rgba(0, 150, 255, 0.6),
    0 10px 25px rgba(0, 212, 255, 0.4);
  border-color: rgba(0, 212, 255, 0.9);
}

.algorithm-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
  transition: left 0.5s ease;
}

.algorithm-card:hover::before {
  left: 100%;
}

.card-glow {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle, rgba(0, 212, 255, 0.1) 0%, transparent 70%);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.algorithm-card:hover .card-glow {
  opacity: 1;
}

.card-content {
  padding: 2.5rem;
  position: relative;
  z-index: 10;
}

.card-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
  display: block;
  animation: iconFloat 3s ease-in-out infinite;
  transition: transform 0.3s ease;
}

.algorithm-card:hover .card-icon {
  transform: scale(1.1);
}

@keyframes iconFloat {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

.algorithm-card h2 {
  font-size: 1.6rem;
  margin-bottom: 1.5rem;
  font-weight: 700;
  background: linear-gradient(90deg, #80d0ff, #a1faff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  transition: all 0.3s ease;
}

.algorithm-card:hover h2 {
  transform: translateY(-3px);
  background: linear-gradient(90deg, #ffffff, #80d0ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.algorithm-card p {
  font-size: 1rem;
  line-height: 1.6;
  margin-bottom: 2rem;
  color: #cceeff;
  font-weight: 400;
  transition: all 0.3s ease;
}

.algorithm-card:hover p {
  color: #ffffff;
  transform: translateY(-2px);
}

.algorithm-card code {
  background: rgba(0, 0, 0, 0.5);
  padding: 0.3rem 0.6rem;
  border-radius: 0.5rem;
  font-family: 'Courier New', monospace;
  color: #ffdf7f;
  font-size: 0.9rem;
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
}

.algorithm-card:hover code {
  background: rgba(0, 0, 0, 0.7);
  color: #fff3a0;
}

.algorithm-card code::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 223, 127, 0.3), transparent);
  animation: codeShimmer 3s ease-in-out infinite;
}

@keyframes codeShimmer {
  0%,
  100% {
    left: -100%;
  }
  50% {
    left: 100%;
  }
}

.animated-btn {
  position: relative;
  background: linear-gradient(135deg, #4facfe, #00f2fe);
  border: none;
  padding: 1rem 2rem;
  border-radius: 1rem;
  cursor: pointer;
  font-weight: 700;
  color: white;
  font-size: 1rem;
  font-family: 'Orbitron', sans-serif;
  box-shadow: 0 4px 15px rgba(0, 242, 254, 0.3);
  overflow: hidden;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  margin: 0 auto;
  /* EVITAR BLUR EN BOTÓN */
  backface-visibility: hidden;
}

.animated-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 25px rgba(0, 242, 254, 0.5);
  background: linear-gradient(135deg, #00f2fe, #4facfe);
}

.btn-icon {
  transition: transform 0.3s ease;
}

.animated-btn:hover .btn-icon {
  transform: translateX(5px);
}

.animated-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
  transition: left 0.5s ease;
}

.animated-btn:hover::before {
  left: 100%;
}

/* Animaciones específicas por card */
.card-1 {
  animation-delay: 0s;
}

.card-2 {
  animation-delay: 0.2s;
}

.card-3 {
  animation-delay: 0.4s;
}

/* Responsive Design */
@media (max-width: 768px) {
  .animated-title {
    font-size: 4rem;
  }

  .animated-subtitle {
    font-size: 1.2rem;
  }

  .cards-container {
    grid-template-columns: 1fr;
    gap: 2rem;
  }

  .card-content {
    padding: 2rem;
  }

  .section-title h2 {
    font-size: 2rem;
  }
}

@media (max-width: 480px) {
  .animated-title {
    font-size: 3rem;
  }

  .animated-subtitle {
    font-size: 1rem;
  }

  .card-content {
    padding: 1.5rem;
  }
}

code {
  text-size-adjust: 100px;
}
</style>
