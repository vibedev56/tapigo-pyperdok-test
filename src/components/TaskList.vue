<template>
  <div class="task-list">
    <h1>Список задач</h1>
    <div v-if="loading">Загрузка...</div>
    <div v-else-if="error">Ошибка: {{ error }}</div>
    <div v-else>
      <div v-for="task in tasks" :key="task.id" class="task-item">
        <input 
          type="checkbox" 
          :id="`task-${task.id}`" 
          v-model="task.done" 
          @change="saveTasksToLocalStorage"
        />
        <label :for="`task-${task.id}`" :class="{ 'completed': task.done }">
          {{ task.title }}
        </label>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const tasks = ref([]);
const loading = ref(true);
const error = ref(null);

const loadTasksFromLocalStorage = () => {
  const savedTasks = localStorage.getItem('tasks');
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks);
    loading.value = false;
    return true;
  }
  return false;
};

const saveTasksToLocalStorage = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
};

const fetchTasks = async () => {
  try {
    const response = await fetch('/tasks.json');
    if (!response.ok) {
      throw new Error('Не удалось загрузить задачи');
    }
    tasks.value = await response.json();
    saveTasksToLocalStorage();
  } catch (err) {
    error.value = err.message;
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  const tasksLoaded = loadTasksFromLocalStorage();
  if (!tasksLoaded) {
    fetchTasks();
  }
});
</script>

<style scoped>
.task-list {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
}

.task-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 4px;
  background-color: #f5f5f5;
}

.task-item label {
  margin-left: 10px;
  cursor: pointer;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}
</style>