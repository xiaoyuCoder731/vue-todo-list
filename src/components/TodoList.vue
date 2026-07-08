<script setup>
import { ref, computed, onMounted } from 'vue';
import TodoItem from './TodoItem.vue';
import TodoForm from './TodoForm.vue';

const todos = ref([]);
const filter = ref('all');

const STORAGE_KEY = 'vue-todo-list';

const loadTodos = () => {
  const saved = localStorage.getItem(STORAGE_KEY);
  if (saved) {
    todos.value = JSON.parse(saved);
  } else {
    todos.value = [
      { id: 1, text: '学习 Vue3 基础', completed: true, time: '09:00' },
      { id: 2, text: '完成待办清单项目', completed: false, time: '14:00' },
      { id: 3, text: '准备答辩演示', completed: false, time: '16:00' }
    ];
    saveTodos();
  }
};

const saveTodos = () => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(todos.value));
};

onMounted(() => {
  loadTodos();
});

const addTodo = ({ text, time }) => {
  const newTodo = {
    id: Date.now(),
    text,
    completed: false,
    time
  };
  todos.value.unshift(newTodo);
  saveTodos();
};

const toggleTodo = (id) => {
  const todo = todos.value.find(t => t.id === id);
  if (todo) {
    todo.completed = !todo.completed;
    saveTodos();
  }
};

const deleteTodo = (id) => {
  todos.value = todos.value.filter(t => t.id !== id);
  saveTodos();
};

const clearCompleted = () => {
  todos.value = todos.value.filter(t => !t.completed);
  saveTodos();
};

const filteredTodos = computed(() => {
  switch (filter.value) {
    case 'active':
      return todos.value.filter(t => !t.completed);
    case 'completed':
      return todos.value.filter(t => t.completed);
    default:
      return todos.value;
  }
});

const stats = computed(() => {
  const total = todos.value.length;
  const completed = todos.value.filter(t => t.completed).length;
  const active = total - completed;
  return { total, completed, active };
});
</script>

<template>
  <div class="todo-container">
    <div class="header">
      <h1>📝 智能待办清单</h1>
      <p class="subtitle">高效管理你的每一天</p>
    </div>

    <TodoForm @add="addTodo" />

    <div class="filters">
      <button
        :class="['filter-btn', { active: filter === 'all' }]"
        @click="filter = 'all'"
      >
        全部
      </button>
      <button
        :class="['filter-btn', { active: filter === 'active' }]"
        @click="filter = 'active'"
      >
        待完成
      </button>
      <button
        :class="['filter-btn', { active: filter === 'completed' }]"
        @click="filter = 'completed'"
      >
        已完成
      </button>
    </div>

    <div class="todo-list">
      <TransitionGroup name="todo">
        <TodoItem
          v-for="todo in filteredTodos"
          :key="todo.id"
          :todo="todo"
          @toggle="toggleTodo"
          @delete="deleteTodo"
        />
      </TransitionGroup>
      
      <div v-if="filteredTodos.length === 0" class="empty-state">
        <div class="empty-icon">🎉</div>
        <p>暂无待办事项</p>
      </div>
    </div>

    <div class="footer">
      <div class="stats">
        <span>总任务: {{ stats.total }}</span>
        <span>待完成: {{ stats.active }}</span>
        <span>已完成: {{ stats.completed }}</span>
      </div>
      <button
        v-if="stats.completed > 0"
        class="clear-btn"
        @click="clearCompleted"
      >
        清除已完成
      </button>
    </div>
  </div>
</template>

<style scoped>
.todo-container {
  max-width: 500px;
  margin: 0 auto;
  padding: 30px;
}

.header {
  text-align: center;
  margin-bottom: 30px;
}

.header h1 {
  font-size: 36px;
  color: white;
  margin: 0;
  text-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.subtitle {
  color: rgba(255, 255, 255, 0.8);
  font-size: 16px;
  margin: 10px 0 0 0;
}

.filters {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}

.filter-btn {
  padding: 8px 20px;
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: none;
  border-radius: 20px;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.filter-btn:hover {
  background: rgba(255, 255, 255, 0.3);
}

.filter-btn.active {
  background: white;
  color: #667eea;
}

.todo-list {
  min-height: 200px;
}

.empty-state {
  text-align: center;
  padding: 40px;
  color: white;
}

.empty-icon {
  font-size: 48px;
  margin-bottom: 15px;
}

.empty-state p {
  margin: 0;
  font-size: 18px;
  opacity: 0.9;
}

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 0;
  border-top: 1px solid rgba(255, 255, 255, 0.2);
  margin-top: 20px;
}

.stats {
  display: flex;
  gap: 20px;
  color: rgba(255, 255, 255, 0.9);
  font-size: 14px;
}

.clear-btn {
  padding: 8px 16px;
  background: rgba(255, 107, 107, 0.9);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.clear-btn:hover {
  background: #ff6b6b;
}

.todo-enter-active,
.todo-leave-active {
  transition: all 0.3s ease;
}

.todo-enter-from {
  opacity: 0;
  transform: translateX(-30px);
}

.todo-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.todo-move {
  transition: transform 0.3s ease;
}
</style>
