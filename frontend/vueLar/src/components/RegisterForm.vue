<template>
  <div class="glassmorphic-container">
    <div class="glassmorphic-card" :class="{ 'form-success': success }">
      <div class="card-glow"></div>
      
      <h2>Регистрация пользователя</h2>
      
      <form @submit.prevent="registerUser">
        <div class="form-group" :class="{ 'has-value': form.name, 'active': activeField === 'name' }">
          <label for="name">Имя</label>
          <input 
            type="text" 
            id="name" 
            v-model="form.name" 
            required
            class="form-control"
            placeholder="Введите ваше имя"
            @focus="activeField = 'name'"
            @blur="activeField = ''"
          >
          <span class="focus-border"></span>
        </div>

        <div class="form-group" :class="{ 'has-value': form.email, 'active': activeField === 'email' }">
          <label for="email">Email</label>
          <input 
            type="email" 
            id="email" 
            v-model="form.email" 
            required
            class="form-control"
            placeholder="Введите ваш email"
            @focus="activeField = 'email'"
            @blur="activeField = ''"
          >
          <span class="focus-border"></span>
        </div>

        <div class="form-group" :class="{ 'has-value': form.password, 'active': activeField === 'password' }">
          <label for="password">Пароль</label>
          <input 
            type="password" 
            id="password" 
            v-model="form.password" 
            required
            class="form-control"
            placeholder="Минимум 8 символов"
            minlength="8"
            @focus="activeField = 'password'"
            @blur="activeField = ''"
          >
          <span class="focus-border"></span>
        </div>

        <button 
          type="submit" 
          class="btn-submit" 
          :disabled="isLoading"
          @mouseenter="btnHover = true"
          @mouseleave="btnHover = false"
        >
          <span class="btn-text">{{ isLoading ? 'Регистрация...' : 'Зарегистрироваться' }}</span>
          <span class="btn-glow" :class="{ 'active': btnHover }"></span>
        </button>

        <div v-if="error" class="error-message">{{ error }}</div>
        <div v-if="success" class="success-message">
          <svg class="checkmark" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 52 52">
            <circle class="checkmark-circle" cx="26" cy="26" r="25" fill="none"/>
            <path class="checkmark-check" fill="none" d="M14.1 27.2l7.1 7.2 16.7-16.8"/>
          </svg>
          <span>{{ success }}</span>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue';
import axios from 'axios';

const form = reactive({
  name: '',
  email: '',
  password: ''
});

const isLoading = ref(false);
const error = ref('');
const success = ref('');
const activeField = ref('');
const btnHover = ref(false);

const registerUser = async () => {
  isLoading.value = true;
  error.value = '';
  success.value = '';
  
  try {
    const response = await axios.post('http://localhost:8000/api/users', form);
    console.log('Пользователь зарегистрирован:', response.data);
    success.value = 'Регистрация прошла успешно!';
    
    // Сбросить форму
    form.name = '';
    form.email = '';
    form.password = '';
  } catch (err) {
    console.error('Ошибка при регистрации:', err);
    if (err.response && err.response.data && err.response.data.message) {
      error.value = err.response.data.message;
    } else {
      error.value = 'Произошла ошибка при регистрации. Попробуйте позже.';
    }
  } finally {
    isLoading.value = false;
  }
};
</script>

<style scoped>
:root {
  --card-bg: rgba(25, 25, 25, 0.2);
  --card-border: rgba(255, 255, 255, 0.05);
  --input-bg: rgba(255, 255, 255, 0.03);
  --input-border: rgba(255, 255, 255, 0.1);
  --input-focus-border: rgba(255, 255, 255, 0.5);
  --text-primary: rgba(255, 255, 255, 1);
  --text-secondary: rgba(255, 255, 255, 0.7);
  --success-color: #4dffa6;
  --error-color: #ff4d4d;
  --card-padding: 40px;
  --field-spacing: 25px;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes highlight {
  0% { box-shadow: 0 0 0 rgba(255, 255, 255, 0); }
  50% { box-shadow: 0 0 30px rgba(255, 255, 255, 0.2); }
  100% { box-shadow: 0 0 0 rgba(255, 255, 255, 0); }
}

@keyframes floating {
  0% { transform: translateY(0) translateX(0); }
  50% { transform: translateY(-10px) translateX(5px); }
  100% { transform: translateY(0) translateX(0); }
}

@keyframes glow {
  0% { opacity: 0.2; }
  50% { opacity: 0.5; }
  100% { opacity: 0.2; }
}

@keyframes checkmarkCircle {
  0% { stroke-dashoffset: 166; }
  100% { stroke-dashoffset: 0; }
}

@keyframes checkmarkCheck {
  0% { stroke-dashoffset: 48; }
  100% { stroke-dashoffset: 0; }
}

@keyframes borderGlow {
  0% { background-position: 0% 0%; }
  100% { background-position: 200% 0%; }
}

.glassmorphic-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 70vh;
  padding: var(--card-padding);
  animation: fadeIn 0.8s ease-out forwards;
}

.glassmorphic-card {
  position: relative;
  width: 100%;
  max-width: 450px;
  background: var(--card-bg);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border: 1px solid var(--card-border);
  border-radius: 20px;
  padding: var(--card-padding);
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
  color: var(--text-primary);
  overflow: hidden;
  transition: all 0.5s cubic-bezier(0.23, 1, 0.32, 1);
  animation: floating 6s ease-in-out infinite;
}

.glassmorphic-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 50px rgba(0, 0, 0, 0.3);
}

.form-success {
  background: rgba(25, 35, 25, 0.3);
  animation: highlight 2s ease-in-out;
}

.card-glow {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(
    circle at center,
    rgba(255, 255, 255, 0.1) 0%,
    rgba(255, 255, 255, 0) 70%
  );
  opacity: 0.2;
  pointer-events: none;
  animation: glow 4s ease-in-out infinite;
  z-index: -1;
}

h2 {
  text-align: center;
  margin-bottom: 30px;
  font-weight: 300;
  letter-spacing: 2px;
  color: var(--text-primary);
  position: relative;
  padding-bottom: 20px;
  font-size: clamp(1.25rem, 4vw, 1.75rem);
}

h2::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 40px;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--input-focus-border), transparent);
}

.form-group {
  margin-bottom: var(--field-spacing);
  position: relative;
  transition: all 0.3s ease;
}

.form-group.active .focus-border {
  transform: scaleX(1);
  opacity: 1;
  background-size: 200% 100%;
  animation: borderGlow 2s linear infinite;
}

.form-group.has-value label, 
.form-group.active label {
  transform: translateY(-20px) scale(0.8);
  color: var(--text-primary);
}

label {
  position: absolute;
  top: 12px;
  left: 15px;
  pointer-events: none;
  color: var(--text-secondary);
  font-weight: 300;
  letter-spacing: 0.5px;
  transition: all 0.3s ease;
  transform-origin: left top;
  font-size: clamp(0.875rem, 3vw, 1rem);
}

.form-control {
  width: 100%;
  padding: 12px 15px;
  background: var(--input-bg);
  border: none;
  border-bottom: 1px solid var(--input-border);
  border-radius: 8px 8px 0 0;
  font-size: clamp(0.875rem, 3vw, 1rem);
  color: var(--text-primary);
  transition: all 0.3s ease;
  position: relative;
  z-index: 1;
}

.form-control:focus {
  outline: none;
  background: rgba(255, 255, 255, 0.05);
}

.form-control:focus ~ label {
  transform: translateY(-20px) scale(0.8);
  color: var(--text-primary);
}

.form-control::placeholder {
  color: transparent;
  transition: color 0.3s ease;
}

.form-control:focus::placeholder,
.form-group.has-value .form-control::placeholder {
  color: var(--text-secondary);
}

.focus-border {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, 
    rgba(255,255,255,0.3), 
    rgba(255,255,255,0.8), 
    rgba(255,255,255,0.3)
  );
  background-size: 200% 100%;
  transform: scaleX(0);
  transition: all 0.3s ease;
  opacity: 0;
}

.btn-submit {
  position: relative;
  width: 100%;
  padding: 14px;
  background: var(--input-bg);
  color: var(--text-primary);
  border: 1px solid var(--input-border);
  border-radius: 30px;
  cursor: pointer;
  font-size: clamp(0.875rem, 3vw, 1rem);
  margin-top: 30px;
  font-weight: 300;
  letter-spacing: 1.5px;
  transition: all 0.4s ease;
  overflow: hidden;
}

.btn-submit:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
}

.btn-submit:active {
  transform: translateY(1px);
}

.btn-submit:disabled {
  background: rgba(150, 150, 150, 0.05);
  border-color: rgba(150, 150, 150, 0.05);
  cursor: not-allowed;
  opacity: 0.6;
}

.btn-text {
  position: relative;
  z-index: 1;
}

.btn-glow {
  position: absolute;
  top: 0;
  left: -100%;
  width: 200%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.15),
    transparent
  );
  transform: skewX(-20deg);
  transition: all 0.4s ease;
  opacity: 0;
}

.btn-glow.active {
  left: 100%;
  opacity: 1;
  transition: all 1s ease;
}

.error-message {
  color: var(--error-color);
  margin-top: 20px;
  text-align: center;
  font-weight: 300;
  letter-spacing: 0.5px;
  animation: fadeIn 0.5s ease forwards;
  font-size: clamp(0.75rem, 3vw, 0.875rem);
}

.success-message {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: var(--success-color);
  margin-top: 20px;
  text-align: center;
  font-weight: 300;
  letter-spacing: 0.5px;
  animation: fadeIn 0.5s ease forwards;
  font-size: clamp(0.75rem, 3vw, 0.875rem);
}

.checkmark {
  width: 50px;
  height: 50px;
  margin-bottom: 15px;
}

.checkmark-circle {
  stroke-dasharray: 166;
  stroke-dashoffset: 166;
  stroke-width: 2;
  stroke: var(--success-color);
  fill: none;
  animation: checkmarkCircle 0.8s ease-in-out forwards 0.2s;
}

.checkmark-check {
  stroke-dasharray: 48;
  stroke-dashoffset: 48;
  stroke-width: 2;
  stroke: var(--success-color);
  fill: none;
  animation: checkmarkCheck 0.5s ease-in-out forwards 1s;
}

/* Адаптивность */
@media (max-width: 768px) {
  :root {
    --card-padding: 30px;
    --field-spacing: 22px;
  }
  
  .glassmorphic-container {
    padding: 20px;
  }
}

@media (max-width: 480px) {
  :root {
    --card-padding: 20px;
    --field-spacing: 20px;
  }
  
  .glassmorphic-container {
    padding: 15px;
  }
  
  .glassmorphic-card {
    padding: var(--card-padding);
  }
  
  h2 {
    margin-bottom: 25px;
    letter-spacing: 1px;
  }
  
  .form-group {
    margin-bottom: 18px;
  }
  
  .form-control {
    padding: 10px 12px;
  }
  
  .btn-submit {
    margin-top: 25px;
    padding: 12px;
  }
}
</style> 