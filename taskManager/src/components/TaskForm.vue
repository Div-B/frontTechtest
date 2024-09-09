<template>
    <form @submit.prevent="handleSubmit">
      <input v-model="name" placeholder="Task Name" required />
      <select v-model="state">
        <option value="not started">Not Started</option>
        <option value="ongoing">Ongoing</option>
        <option value="completed">Completed</option>
      </select>
      <textarea v-model="description" placeholder="Description"></textarea>
      <button type="submit">Create Task</button>
    </form>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    emits: ['add-task'],
    setup(_, { emit }) {
      const name = ref('');
      const state = ref('not started');
      const description = ref('');
  
      const handleSubmit = () => {
        emit('add-task', { name: name.value, state: state.value, description: description.value });
        name.value = '';
        state.value = 'not started';
        description.value = '';
      };
  
      return { name, state, description, handleSubmit };
    }
  };
  </script>
  
  <style>
  form {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  input, select, textarea, button {
    margin: 5px;
  }
  </style>
  
  