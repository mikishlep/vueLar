<template>
  <div class="register-form">
    <h2>Регистрация пользователя</h2>
    <form @submit.prevent="registerUser">
      <div class="form-group">
        <label for="name">Имя:</label>
        <input 
          type="text" 
          id="name" 
          v-model="form.name" 
          required
          class="form-control"
          placeholder="Введите ваше имя"
        >
      </div>

      <div class="form-group">
        <label for="email">Email:</label>
        <input 
          type="email" 
          id="email" 
          v-model="form.email" 
          required
          class="form-control"
          placeholder="Введите ваш email"
        >
      </div>

      <div class="form-group">
        <label for="password">Пароль:</label>
        <input 
          type="password" 
          id="password" 
          v-model="form.password" 
          required
          class="form-control"
          placeholder="Введите пароль (минимум 8 символов)"
          minlength="8"
        >
      </div>

      <button type="submit" class="btn-submit" :disabled="isLoading">
        {{ isLoading ? 'Регистрация...' : 'Зарегистрироваться' }}
      </button>

      <div v-if="error" class="error-message">{{ error }}</div>
      <div v-if="success" class="success-message">{{ success }}</div>
    </form>
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
.register-form {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

h2 {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
  color: #333;
}

.form-control {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
}

.btn-submit {
  width: 100%;
  padding: 12px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  margin-top: 10px;
}

.btn-submit:hover {
  background-color: #45a049;
}

.btn-submit:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.error-message {
  color: #f44336;
  margin-top: 15px;
  text-align: center;
}

.success-message {
  color: #4CAF50;
  margin-top: 15px;
  text-align: center;
}
</style> 