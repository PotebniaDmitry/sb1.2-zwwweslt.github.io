<script setup lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';
import type { Task } from '../types/task';

const tasks = ref<Task[]>([]);

const fetchTasks = async () => {
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/todos?_limit=10');
    tasks.value = response.data;
  } catch (error) {
    console.error('Error fetching tasks:', error);
  }
};

const toggleTask = async (task: Task) => {
  try {
    await axios.patch(`https://jsonplaceholder.typicode.com/todos/${task.id}`, {
      completed: !task.completed
    });
    task.completed = !task.completed;
  } catch (error) {
    console.error('Error toggling task:', error);
  }
};

const deleteTask = async (taskId: number) => {
  try {
    await axios.delete(`https://jsonplaceholder.typicode.com/todos/${taskId}`);
    tasks.value = tasks.value.filter(task => task.id !== taskId);
  } catch (error) {
    console.error('Error deleting task:', error);
  }
};

onMounted(fetchTasks);
</script>

<template>
  <div class="tasks-list">
    <h2>Tasks List</h2>
    <ul>
      <li v-for="task in tasks" :key="task.id" class="task-item">
        <div class="task-content">
          <input
            type="checkbox"
            :checked="task.completed"
            @change="toggleTask(task)"
          />
          <span :class="{ completed: task.completed }">{{ task.title }}</span>
        </div>
        <button @click="deleteTask(task.id)" class="delete-btn">Delete</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.tasks-list {
  max-width: 600px;
  margin: 0 auto;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  margin: 5px 0;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.task-content {
  display: flex;
  align-items: center;
  gap: 10px;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

.delete-btn {
  background-color: #ff4444;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
}

.delete-btn:hover {
  background-color: #cc0000;
}
</style>