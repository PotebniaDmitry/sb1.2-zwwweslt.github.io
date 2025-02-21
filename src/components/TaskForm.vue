<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';
import type { Task } from '../types/task';

const emit = defineEmits(['taskAdded']);

const newTask = ref('');

const addTask = async () => {
  if (!newTask.value.trim()) return;

  try {
    const response = await axios.post('https://jsonplaceholder.typicode.com/todos', {
      title: newTask.value,
      completed: false,
      userId: 1
    });

    emit('taskAdded', response.data);
    newTask.value = '';
  } catch (error) {
    console.error('Error adding task:', error);
  }
};
</script>

<template>
  <div class="task-form">
    <h2>Add New Task</h2>
    <form @submit.prevent="addTask">
      <input
        v-model="newTask"
        type="text"
        placeholder="Enter new task..."
        required
      />
      <button type="submit">Add Task</button>
    </form>
  </div>
</template>

<style scoped>
.task-form {
  max-width: 600px;
  margin: 20px auto;
}

form {
  display: flex;
  gap: 10px;
}

input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

button {
  padding: 8px 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
</style>