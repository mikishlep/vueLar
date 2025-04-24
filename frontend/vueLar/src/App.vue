<script setup>
import RegisterForm from './components/RegisterForm.vue'
</script>

<template>
  <div class="app">
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

<style>
.app {
  font-family: Arial, sans-serif;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

header {
  text-align: center;
  margin-bottom: 30px;
}

header h1 {
  color: #333;
}

main {
  min-height: 70vh;
}

footer {
  margin-top: 40px;
  text-align: center;
  color: #666;
  padding: 10px;
  border-top: 1px solid #eee;
}
</style>
