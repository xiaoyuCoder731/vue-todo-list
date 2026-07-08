<script setup>
import { ref } from 'vue';

const emit = defineEmits(['add']);

const inputText = ref('');

const handleSubmit = () => {
  const text = inputText.value.trim();
  if (!text) return;
  
  const now = new Date();
  const time = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
  
  emit('add', {
    text,
    time
  });
  
  inputText.value = '';
};

const handleKeydown = (e) => {
  if (e.key === 'Enter') {
    handleSubmit();
  }
};
</script>

<template>
  <div class="todo-form">
    <input
      v-model="inputText"
      type="text"
      placeholder="添加新的待办事项..."
      class="todo-input"
      @keydown="handleKeydown"
    />
    <button class="add-btn" @click="handleSubmit">添加</button>
  </div>
</template>

<style scoped>
.todo-form {
  display: flex;
  gap: 12px;
  margin-bottom: 25px;
}

.todo-input {
  flex: 1;
  padding: 15px 20px;
  border: 2px solid #e0e0e0;
  border-radius: 12px;
  font-size: 16px;
  transition: all 0.3s ease;
  outline: none;
}

.todo-input:focus {
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.todo-input::placeholder {
  color: #bbb;
}

.add-btn {
  padding: 15px 30px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
}

.add-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

.add-btn:active {
  transform: translateY(0);
}
</style>
