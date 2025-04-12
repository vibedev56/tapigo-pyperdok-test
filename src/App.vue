<template>
  <div class="app">
    <TaskList />
  </div>
</template>

<script setup>
import TaskList from './components/TaskList.vue';
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f9f9f9;
}

.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}
</style>
