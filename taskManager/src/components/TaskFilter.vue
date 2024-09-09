<template>
    <div>
      <button @click="filter('all')">All</button>
      <button @click="filter('not started')">Not Started</button>
      <button @click="filter('ongoing')">Ongoing</button>
      <button @click="filter('completed')">Completed</button>
    </div>
  </template>
  
  <script>
  export default {
    emits: ['filter-tasks'],
    methods: {
      filter(state) {
        this.$emit('filter-tasks', state);
      }
    }
  };
  </script>
  
  