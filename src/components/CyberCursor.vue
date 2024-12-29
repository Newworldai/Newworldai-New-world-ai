<template>
  <div class="cursor-container" ref="cursorContainer">
    <div class="cursor-main" ref="cursorMain"></div>
    <div class="cursor-trail" ref="cursorTrail"></div>
    <div class="cursor-glow" ref="cursorGlow"></div>
    <div class="click-effects"></div>
  </div>
</template>

<script>
export default {
  name: 'CyberCursor',
  mounted() {
    const cursorMain = this.$refs.cursorMain
    const cursorTrail = this.$refs.cursorTrail
    const cursorGlow = this.$refs.cursorGlow
    const cursorContainer = this.$refs.cursorContainer
    
    document.addEventListener('mousemove', (e) => {
      const { clientX, clientY } = e
      cursorMain.style.transform = `translate(${clientX}px, ${clientY}px)`
      setTimeout(() => {
        cursorTrail.style.transform = `translate(${clientX}px, ${clientY}px)`
      }, 50)
      cursorGlow.style.transform = `translate(${clientX}px, ${clientY}px)`
      
      
      cursorContainer.style.setProperty('--x', `${clientX}px`)
      cursorContainer.style.setProperty('--y', `${clientY}px`)
    })

    document.addEventListener('mousedown', () => {
      cursorContainer.classList.add('clicking')
      this.createClickEffect(event)
    })

    document.addEventListener('mouseup', () => {
      cursorContainer.classList.remove('clicking')
    })
  },
  methods: {
    createClickEffect(e) {
      const { clientX, clientY } = e
      const effectsContainer = document.querySelector('.click-effects')
      

      const flash = document.createElement('div')
      flash.className = 'click-flash'
      flash.style.left = `${clientX}px`
      flash.style.top = `${clientY}px`
      effectsContainer.appendChild(flash)

      
      for (let i = 0; i < 3; i++) {
        const ring = document.createElement('div')
        ring.className = `click-ring ring-${i}`
        ring.style.left = `${clientX}px`
        ring.style.top = `${clientY}px`
        effectsContainer.appendChild(ring)

        ring.addEventListener('animationend', () => {
          effectsContainer.removeChild(ring)
        })
      }

      flash.addEventListener('animationend', () => {
        effectsContainer.removeChild(flash)
      })
    }
  }
}
</script>

<style scoped>
.cursor-container {
  pointer-events: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 9999;
}

.cursor-main {
  position: fixed;
  width: 8px;
  height: 8px;
  background: #0ff;
  border-radius: 50%;
  transform: translate(-50%, -50%);
  mix-blend-mode: difference;
}

.cursor-trail {
  position: fixed;
  width: 16px;
  height: 16px;
  border: 1px solid rgba(0, 255, 255, 0.5);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: transform 0.15s ease;
  animation: trail-pulse 2s infinite;
}

.cursor-glow {
  position: fixed;
  width: 40px;
  height: 40px;
  background: radial-gradient(circle, rgba(0, 255, 255, 0.3) 0%, transparent 70%);
  transform: translate(-50%, -50%);
  transition: transform 0.2s ease;
  animation: glow-pulse 2s infinite alternate;
}

.click-effects {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 9998;
}

.click-flash {
  position: absolute;
  width: 20px;
  height: 20px;
  background: #0ff;
  border-radius: 50%;
  transform: translate(-50%, -50%);
  mix-blend-mode: screen;
  animation: flash 0.5s ease-out forwards;
}

.click-ring {
  position: absolute;
  border-radius: 50%;
  transform: translate(-50%, -50%);
}

.ring-0 {
  animation: ring1 0.6s ease-out forwards;
  border: 3px solid #0ff;
  box-shadow: 0 0 20px #0ff, inset 0 0 20px #0ff;
}

.ring-1 {
  animation: ring2 0.8s ease-out forwards;
  border: 2px solid rgba(0, 255, 255, 0.8);
  box-shadow: 0 0 15px rgba(0, 255, 255, 0.8);
}

.ring-2 {
  animation: ring3 1s ease-out forwards;
  border: 1px solid rgba(0, 255, 255, 0.6);
  box-shadow: 0 0 10px rgba(0, 255, 255, 0.6);
}

@keyframes flash {
  0% {
    transform: translate(-50%, -50%) scale(1);
    opacity: 1;
    box-shadow: 0 0 40px #0ff, 0 0 60px #0ff, 0 0 80px #0ff;
  }
  100% {
    transform: translate(-50%, -50%) scale(3);
    opacity: 0;
    box-shadow: 0 0 80px transparent, 0 0 120px transparent, 0 0 160px transparent;
  }
}

@keyframes ring1 {
  0% {
    width: 0;
    height: 0;
    opacity: 1;
  }
  100% {
    width: 200px;
    height: 200px;
    opacity: 0;
  }
}

@keyframes ring2 {
  0% {
    width: 0;
    height: 0;
    opacity: 0.8;
  }
  100% {
    width: 300px;
    height: 300px;
    opacity: 0;
  }
}

@keyframes ring3 {
  0% {
    width: 0;
    height: 0;
    opacity: 0.6;
  }
  100% {
    width: 400px;
    height: 400px;
    opacity: 0;
  }
}

.cursor-container.clicking .cursor-main {
  transform: translate(-50%, -50%) scale(0.8);
}

.cursor-container.clicking .cursor-trail {
  transform: translate(-50%, -50%) scale(1.2);
}
</style> 