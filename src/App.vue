<script setup>
import { ref } from 'vue';
import Auth from './components/Auth.vue';
import TodoList from './components/TodoList.vue';

const currentUser = ref(null);

const handleLogin = (user) => {
  currentUser.value = user;
};

const handleLogout = () => {
  currentUser.value = null;
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
