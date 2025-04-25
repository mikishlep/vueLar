<script setup>
import RegisterForm from './components/RegisterForm.vue'
import { onMounted, onUnmounted, ref } from 'vue'

const mouseX = ref(0)
const mouseY = ref(0)

function handleMouseMove(e) {
  mouseX.value = e.clientX
  mouseY.value = e.clientY
  
  const circles = document.querySelectorAll('.circle')
  circles.forEach((circle, index) => {
    // Разная скорость движения для каждого круга
    const speed = 0.02 - (index * 0.003)
    // Расчет новой позиции
    const x = (e.clientX - window.innerWidth / 2) * speed
    const y = (e.clientY - window.innerHeight / 2) * speed
    
    // Применение трансформации с задержкой
    circle.style.transform = `translate(${x}px, ${y}px) scale(${1 + (index * 0.01)})`
  })
}

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove)
})

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove)
})
</script>

<template>
  <div class="app">
    <div class="animated-background">
      <div class="circle circle-1" @mouseover="e => circlePulse(e, 1)" @mouseout="e => circleReset(e, 1)"></div>
      <div class="circle circle-2" @mouseover="e => circlePulse(e, 2)" @mouseout="e => circleReset(e, 2)"></div>
      <div class="circle circle-3" @mouseover="e => circlePulse(e, 3)" @mouseout="e => circleReset(e, 3)"></div>
      <div class="circle circle-4" @mouseover="e => circlePulse(e, 4)" @mouseout="e => circleReset(e, 4)"></div>
      <div class="circle circle-5" @mouseover="e => circlePulse(e, 5)" @mouseout="e => circleReset(e, 5)"></div>
    </div>
    
    <header>
      <h1>Система управления пользователями</h1>
    </header>
    
    <main>
      <RegisterForm />
    </main>
    
    <footer>
      <p>&copy; 2025 Система управления пользователями</p>
    </footer>
  </div>
</template>

<script>
function circlePulse(e, index) {
  const circle = e.target
  circle.classList.add('pulse')
  circle.style.background = `rgba(255, 255, 255, 0.${index + 2})`
  circle.style.transform = 'scale(1.2)'
}

function circleReset(e, index) {
  const circle = e.target
  circle.classList.remove('pulse')
  circle.style.background = 'rgba(255, 255, 255, 0.03)'
  circle.style.transform = 'scale(1)'
}
</script>

<style>
@keyframes float {
  0% { transform: translateY(0) rotate(0deg); }
  50% { transform: translateY(-20px) rotate(5deg); }
  100% { transform: translateY(0) rotate(0deg); }
}

@keyframes pulse {
  0% { transform: scale(1); opacity: 0.3; }
  50% { transform: scale(1.3); opacity: 0.6; }
  100% { transform: scale(1); opacity: 0.3; }
}

@keyframes rotateBackground {
  0% { background-position: 0% 0%; }
  100% { background-position: 100% 100%; }
}

@keyframes circlePulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.2); }
  100% { transform: scale(1); }
}

:root {
  --text-primary: rgba(255, 255, 255, 0.95);
  --text-secondary: rgba(255, 255, 255, 0.7);
  --bg-dark: #0a0a0a;
  --bg-light: rgba(255, 255, 255, 0.03);
  --border-light: rgba(255, 255, 255, 0.1);
}

html {
  font-size: 16px;
}

body {
  margin: 0;
  padding: 0;
  background: var(--bg-dark);
  color: var(--text-primary);
  font-family: 'Helvetica Neue', Arial, sans-serif;
  min-height: 100vh;
  overflow-x: hidden;
}

.app {
  position: relative;
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px 20px;
  z-index: 1;
}

.animated-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  background: linear-gradient(145deg, #121212, #0a0a0a);
  background-size: 400% 400%;
  animation: rotateBackground 30s ease infinite;
  overflow: hidden;
}

.circle {
  position: absolute;
  border-radius: 50%;
  background: var(--bg-light);
  backdrop-filter: blur(5px);
  -webkit-backdrop-filter: blur(5px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  border: 1px solid var(--border-light);
  transition: all 0.5s cubic-bezier(0.23, 1, 0.32, 1);
  cursor: pointer;
}

.circle.pulse {
  animation: circlePulse 2s infinite ease-in-out;
}

.circle-1 {
  width: 300px;
  height: 300px;
  top: 10%;
  left: 10%;
}

.circle-2 {
  width: 200px;
  height: 200px;
  top: 60%;
  left: 15%;
}

.circle-3 {
  width: 400px;
  height: 400px;
  top: 40%;
  right: 10%;
}

.circle-4 {
  width: 150px;
  height: 150px;
  top: 20%;
  right: 20%;
}

.circle-5 {
  width: 250px;
  height: 250px;
  bottom: 10%;
  right: 30%;
}

header {
  text-align: center;
  margin-bottom: 40px;
  padding: 20px 0;
}

header h1 {
  color: var(--text-primary);
  font-weight: 300;
  letter-spacing: 3px;
  margin: 0;
  position: relative;
  display: inline-block;
  padding-bottom: 10px;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
  font-size: clamp(1.5rem, 5vw, 2.5rem);
}

header h1::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 60%;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.5), transparent);
}

main {
  min-height: 70vh;
  position: relative;
  z-index: 1;
}

footer {
  margin-top: 60px;
  text-align: center;
  color: var(--text-secondary);
  padding: 20px 0;
  font-weight: 300;
  letter-spacing: 1px;
  font-size: clamp(0.75rem, 2vw, 0.875rem);
  position: relative;
}

footer::before {
  content: '';
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 30%;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
}

/* Адаптивность для мобильных устройств */
@media (max-width: 768px) {
  .app {
    padding: 20px 15px;
  }
  
  .circle-1 {
    width: 200px;
    height: 200px;
  }
  
  .circle-2 {
    width: 120px;
    height: 120px;
  }
  
  .circle-3 {
    width: 250px;
    height: 250px;
  }
  
  .circle-4 {
    width: 100px;
    height: 100px;
  }
  
  .circle-5 {
    width: 150px;
    height: 150px;
  }
  
  header {
    margin-bottom: 20px;
  }
  
  footer {
    margin-top: 40px;
  }
}

@media (max-width: 480px) {
  .app {
    padding: 15px 10px;
  }
  
  .circle-1, .circle-3 {
    opacity: 0.5;
  }
  
  header h1 {
    letter-spacing: 2px;
  }
}
</style>
