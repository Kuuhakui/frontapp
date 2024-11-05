<template>
    <div class="login-page">
      <h1>Страница авторизации</h1>
      <form @submit.prevent="handleLogin">
        <div class="form-group">
          <label for="username">Имя пользователя:</label>
          <input type="text" id="username" v-model="username"/>
        </div>
        <div class="form-group">
          <label for="password">Пароль:</label>
          <input type="password" id="password" v-model="password" />
        </div>
        <button type="submit">Войти</button>
      </form>
    </div>
  </template>


<script>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

export default {
  name: 'LoginPage',
  setup() {
    const router = useRouter();
    const username = ref('');
    const password = ref('');

    const handleLogin = () => {
      // Простая проверка, чтобы эмулировать процесс авторизации
      if (username.value && password.value) {
        localStorage.setItem('isAuthenticated', true);
        router.push('/home');
      } else {
        alert('Введите имя пользователя и пароль');
      }
    };

    return {
      username,
      password,
      handleLogin
    };
  }
};
</script>
<style scoped>
.login-page {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  background-color: #ffffff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  font-family: Arial, sans-serif;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 4px;
}
button {
  width: 100%;
  padding: 10px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>
