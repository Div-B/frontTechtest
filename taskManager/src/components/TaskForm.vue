<template>
    <form @submit.prevent="handleSubmit" id="taskform">
      <input class="taskforminput" v-model="name" placeholder="Name your task" required />
      
      <select class="taskformselect" v-model="state">
        <option value="choosestate">Choose State</option>  
        <option value="not started">Not Started</option>
        <option value="ongoing">Ongoing</option>
        <option value="completed">Completed</option>
      </select>
      <textarea class="taskformtextarea" v-model="description" placeholder="Write a description"></textarea>
      <button class="taskformbutton" type="submit">Submit</button>
    </form>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    emits: ['add-task'],
    setup(_, { emit }) {
      const name = ref('');
      const state = ref('choosestate');
      const description = ref('');
  
      const handleSubmit = () => {
        emit('add-task', { name: name.value, state: state.value, description: description.value });
        name.value = '';
        state.value = 'choosestate';
        description.value = '';
      };
  
      return { name, state, description, handleSubmit };
    }
  };
  </script>
  
  
  <style src="./src/style.css"></style>