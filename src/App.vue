<script setup>
import { ref } from 'vue';
import Auth from './components/Auth.vue';
import TodoList from './components/TodoList.vue';

const CURRENT_USER_KEY = 'vue-todo-current-user';

const savedUser = localStorage.getItem(CURRENT_USER_KEY);
const currentUser = ref(savedUser ? JSON.parse(savedUser) : null);

const handleLogin = (user) => {
  currentUser.value = user;
  localStorage.setItem(CURRENT_USER_KEY, JSON.stringify(user));
};

const handleLogout = () => {
  currentUser.value = null;
  localStorage.removeItem(CURRENT_USER_KEY);
};
</script>

<template>
  <div class="app">
    <Auth v-if="!currentUser" @login="handleLogin" />
    <TodoList v-else :user="currentUser" @logout="handleLogout" />
  </div>
</template>

<style scoped>
.app {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
</style>
