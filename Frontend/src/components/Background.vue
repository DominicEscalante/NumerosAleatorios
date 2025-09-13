<template>
  <div>
    <div id="tsparticles" ref="particlesContainer"></div>
  </div>
</template>

<script setup>
import { onMounted, ref, onUnmounted } from 'vue'

const particlesContainer = ref(null)
let particlesInstance = null

const particleOptions = {
  background: {
    color: {
      value: '#000000',
    },
  },
  fullScreen: {
    enable: true,
    zIndex: -1,
  },
  fpsLimit: 120,
  interactivity: {
    events: {
      onClick: {
        enable: true,
        mode: 'push',
      },
      onHover: {
        enable: true,
        mode: 'repulse',
      },
      resize: true,
    },
    modes: {
      bubble: {
        distance: 400,
        duration: 2,
        opacity: 0.8,
        size: 40,
      },
      push: {
        quantity: 4,
      },
      repulse: {
        distance: 150,
        duration: 0.4,
      },
    },
  },
  particles: {
    color: {
      value: '#5591a9',
    },
    links: {
      color: '#5591a9',
      distance: 150,
      enable: true,
      opacity: 0.5,
      width: 1,
    },
    collisions: {
      enable: true,
    },
    move: {
      direction: 'none',
      enable: true,
      outModes: {
        default: 'bounce',
      },
      random: false,
      speed: 2,
      straight: false,
    },
    number: {
      density: {
        enable: true,
        area: 800,
      },
      value: 150,
    },
    opacity: {
      value: 0.5,
    },
    shape: {
      type: 'circle',
    },
    size: {
      value: { min: 1, max: 5 },
    },
  },
  detectRetina: true,
}

const loadScript = (src) => {
  return new Promise((resolve, reject) => {
    if (document.querySelector(`script[src="${src}"]`)) {
      resolve()
      return
    }

    const script = document.createElement('script')
    script.src = src
    script.onload = resolve
    script.onerror = reject
    document.head.appendChild(script)
  })
}

onMounted(async () => {
  try {
    await loadScript(
      'https://cdn.jsdelivr.net/npm/tsparticles-engine@2.12.0/tsparticles.engine.min.js',
    )
    await loadScript('https://cdn.jsdelivr.net/npm/tsparticles@2.12.0/tsparticles.bundle.min.js')

    await new Promise((resolve) => setTimeout(resolve, 100))

    if (window.tsParticles) {
      particlesInstance = await window.tsParticles.load('tsparticles', particleOptions)
      console.log('Particulas cargadas correctamente')
    } else {
      console.error('tsParticles no está disponible')
    }
  } catch (error) {
    console.error('Error al cargar tsParticles:', error)
  }
})

onUnmounted(() => {
  if (particlesInstance) {
    particlesInstance.destroy()
  }
})
</script>

<style scoped>
#tsparticles {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
}
</style>
