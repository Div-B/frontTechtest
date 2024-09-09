<template>
  <div id="app">
    <header>
      <span v-if="userName">Welcome, {{ userName }}!</span>
    </header>
    <div class="form-container">
      <UserForm @set-user="setUserName" />
    </div>
    <div class="menu-container">
      <button @click="setView('create')">Create a Task</button>
      <button @click="setView('list')">List Tasks</button>
    </div>
    <div v-if="view === 'create'" class="task-form-container">
      <TaskForm @add-task="addTask" />
    </div>
    <div v-if="view === 'list'" class="task-list-container">
      <div class="filter-menu">
        <label for="filter-tasks">Filter tasks:</label>
        <select id="filter-tasks" v-model="selectedFilter" @change="applyFilter">
          <option value="all">All</option>
          <option value="not started">Not Started</option>
          <option value="ongoing">Ongoing</option>
          <option value="completed">Completed</option>
        </select>
      </div>
      <TaskList
        :tasks="filteredTasks"
        @delete-task="deleteTask"
        @edit-task="editTask"
      />
    </div>
  </div>
</template>

<script>
import { ref, onMounted, computed, watch } from 'vue';
import UserForm from './components/UserForm.vue';
import TaskForm from './components/TaskForm.vue';
import TaskList from './components/TaskList.vue';

export default {
  components: { UserForm, TaskForm, TaskList },
  setup() {
    const userName = ref(localStorage.getItem('userName') || '');
    const tasks = ref([]);
    const selectedFilter = ref('all');
    const view = ref('list'); // Default view set to 'list'

    const setUserName = (name) => {
      userName.value = name;
      localStorage.setItem('userName', name);
      resetTasks();
    };

    const setView = (newView) => {
      view.value = newView;
    };

    const addTask = (task) => {
      tasks.value.push({ ...task, id: Date.now(), createdAt: new Date() });
      saveTasks();
    };

    const deleteTask = (id) => {
      tasks.value = tasks.value.filter(task => task.id !== id);
      saveTasks();
    };

    const editTask = (updatedTask) => {
      const index = tasks.value.findIndex(task => task.id === updatedTask.id);
      if (index !== -1) {
        tasks.value[index] = updatedTask;
        saveTasks();
      }
    };

    const applyFilter = () => {
      // Logic to filter tasks based on selectedFilter
    };

    const saveTasks = () => {
      localStorage.setItem(`tasks_${userName.value}`, JSON.stringify(tasks.value));
    };

    const loadTasks = () => {
      const savedTasks = JSON.parse(localStorage.getItem(`tasks_${userName.value}`));
      if (savedTasks) {
        tasks.value = savedTasks;
      }
    };

    const resetTasks = () => {
      tasks.value = [];
      localStorage.removeItem(`tasks_${userName.value}`);
    };

    onMounted(loadTasks);

    watch(userName, loadTasks);

    const filteredTasks = computed(() => {
      if (selectedFilter.value === 'all') return tasks.value;
      return tasks.value.filter(task => task.state === selectedFilter.value);
    });

    return { userName, setUserName, tasks, addTask, deleteTask, editTask, filteredTasks, view, setView, selectedFilter, applyFilter };
  }
};
</script>

<style>
header {
  position: absolute;
  top: 10px;
  right: 10px;
}
.form-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  height: 100vh;
  padding-top: 20px;
}
.menu-container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
.menu-container button {
  margin: 0 10px;
}
.task-form-container, .task-list-container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
.filter-menu {
  margin-bottom: 10px;
}
</style>




