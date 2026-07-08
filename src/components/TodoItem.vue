<script setup>
defineProps({
  todo: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['toggle', 'delete']);
</script>

<template>
  <div :class="['todo-item', { completed: todo.completed }]">
    <div class="checkbox" @click="emit('toggle', todo.id)">
      <span v-if="todo.completed">✓</span>
    </div>
    <span class="text">{{ todo.text }}</span>
    <span class="time">{{ todo.time }}</span>
    <button class="delete-btn" @click="emit('delete', todo.id)">×</button>
  </div>
</template>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  padding: 15px 20px;
  background: white;
  border-radius: 12px;
  margin-bottom: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
  transition: all 0.3s ease;
}

.todo-item:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.todo-item.completed {
  opacity: 0.6;
}

.todo-item.completed .text {
  text-decoration: line-through;
  color: #999;
}

.checkbox {
  width: 24px;
  height: 24px;
  border: 2px solid #ddd;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  margin-right: 15px;
  transition: all 0.3s ease;
  flex-shrink: 0;
}

.todo-item:hover .checkbox {
  border-color: #667eea;
}

.todo-item.completed .checkbox {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-color: #667eea;
  color: white;
}

.text {
  flex: 1;
  font-size: 16px;
  color: #333;
}

.time {
  font-size: 12px;
  color: #999;
  margin-right: 15px;
}

.delete-btn {
  width: 32px;
  height: 32px;
  border: none;
  background: #f5f5f5;
  border-radius: 50%;
  font-size: 20px;
  color: #999;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  flex-shrink: 0;
}

.delete-btn:hover {
  background: #ff6b6b;
  color: white;
}
</style>
