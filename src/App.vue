<script setup lang="ts">
import TasksList from './components/TasksList.vue'
import TaskForm from './components/TaskForm.vue'
</script>

<template>
  <div class="app-container">
    <h1>Todo Application</h1>
    <TaskForm />
    <TasksList />
  </div>
</template>

<style>
.app-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  color: #333;
}
</style>