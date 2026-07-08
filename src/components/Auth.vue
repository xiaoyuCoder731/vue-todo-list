<script setup>
import { ref } from 'vue';

const emit = defineEmits(['login']);

const isLogin = ref(true);
const username = ref('');
const password = ref('');
const confirmPassword = ref('');
const errorMessage = ref('');

const USERS_KEY = 'vue-todo-users';

const getUsers = () => {
  const saved = localStorage.getItem(USERS_KEY);
  return saved ? JSON.parse(saved) : [];
};

const saveUsers = (users) => {
  localStorage.setItem(USERS_KEY, JSON.stringify(users));
};

const handleLogin = () => {
  errorMessage.value = '';
  
  if (!username.value.trim() || !password.value) {
    errorMessage.value = '请输入用户名和密码';
    return;
  }
  
  const users = getUsers();
  const user = users.find(u => u.username === username.value.trim());
  
  if (!user) {
    errorMessage.value = '用户名不存在';
    return;
  }
  
  if (user.password !== password.value) {
    errorMessage.value = '密码错误';
    return;
  }
  
  emit('login', user);
};

const handleRegister = () => {
  errorMessage.value = '';
  
  if (!username.value.trim()) {
    errorMessage.value = '请输入用户名';
    return;
  }
  
  if (!password.value) {
    errorMessage.value = '请输入密码';
    return;
  }
  
  if (password.value !== confirmPassword.value) {
    errorMessage.value = '两次输入的密码不一致';
    return;
  }
  
  if (password.value.length < 6) {
    errorMessage.value = '密码长度至少6位';
    return;
  }
  
  const users = getUsers();
  const exists = users.find(u => u.username === username.value.trim());
  
  if (exists) {
    errorMessage.value = '用户名已存在';
    return;
  }
  
  const newUser = {
    id: Date.now(),
    username: username.value.trim(),
    password: password.value,
    createdAt: new Date().toLocaleString()
  };
  
  users.push(newUser);
  saveUsers(users);
  
  emit('login', newUser);
};

const toggleMode = () => {
  isLogin.value = !isLogin.value;
  errorMessage.value = '';
  username.value = '';
  password.value = '';
  confirmPassword.value = '';
};
</script>

<template>
  <div class="auth-container">
    <div class="auth-card">
      <div class="auth-header">
        <div class="logo">📝</div>
        <h1>{{ isLogin ? '欢迎回来' : '创建账号' }}</h1>
        <p>{{ isLogin ? '登录您的账户' : '注册新账户' }}</p>
      </div>

      <div class="auth-form">
        <div v-if="errorMessage" class="error-message">
          {{ errorMessage }}
        </div>

        <div class="form-group">
          <label>用户名</label>
          <input
            v-model="username"
            type="text"
            placeholder="请输入用户名"
            class="form-input"
          />
        </div>

        <div class="form-group">
          <label>密码</label>
          <input
            v-model="password"
            type="password"
            placeholder="请输入密码"
            class="form-input"
          />
        </div>

        <div v-if="!isLogin" class="form-group">
          <label>确认密码</label>
          <input
            v-model="confirmPassword"
            type="password"
            placeholder="请再次输入密码"
            class="form-input"
          />
        </div>

        <button
          class="auth-btn"
          @click="isLogin ? handleLogin() : handleRegister()"
        >
          {{ isLogin ? '登 录' : '注 册' }}
        </button>
      </div>

      <div class="auth-footer">
        <span>{{ isLogin ? '还没有账号？' : '已有账号？' }}</span>
        <button class="link-btn" @click="toggleMode">
          {{ isLogin ? '立即注册' : '立即登录' }}
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.auth-container {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 20px;
}

.auth-card {
  width: 100%;
  max-width: 420px;
  background: white;
  border-radius: 24px;
  padding: 40px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15);
}

.auth-header {
  text-align: center;
  margin-bottom: 35px;
}

.logo {
  font-size: 60px;
  margin-bottom: 15px;
}

.auth-header h1 {
  font-size: 28px;
  color: #1e293b;
  margin: 0 0 8px 0;
}

.auth-header p {
  color: #94a3b8;
  margin: 0;
}

.auth-form {
  margin-bottom: 25px;
}

.error-message {
  background: #fee2e2;
  color: #dc2626;
  padding: 12px 16px;
  border-radius: 8px;
  margin-bottom: 20px;
  font-size: 14px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  font-size: 14px;
  font-weight: 500;
  color: #475569;
  margin-bottom: 8px;
}

.form-input {
  width: 100%;
  padding: 14px 16px;
  border: 2px solid #e2e8f0;
  border-radius: 12px;
  font-size: 16px;
  transition: all 0.3s ease;
  outline: none;
}

.form-input:focus {
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.form-input::placeholder {
  color: #94a3b8;
}

.auth-btn {
  width: 100%;
  padding: 16px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.auth-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

.auth-footer {
  text-align: center;
  color: #64748b;
  font-size: 14px;
}

.link-btn {
  background: none;
  border: none;
  color: #667eea;
  font-weight: 600;
  cursor: pointer;
  font-size: 14px;
  margin-left: 5px;
  transition: color 0.3s ease;
}

.link-btn:hover {
  color: #5a6fd6;
}
</style>
