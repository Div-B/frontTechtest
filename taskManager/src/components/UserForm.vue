<template>
    <div class="task-list" @click="hideContextMenu">
      <table>
        <thead>
          <tr>
            <th>Task created on</th>
            <th>Task Name</th>
            <th>State</th>
            <th>Description</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="task in tasks" :key="task.id" @contextmenu.prevent="showContextMenu($event, task)">
            <td>{{ formatDate(task.createdAt) }}</td>
            <td v-if="task.isEditing">
              <input v-model="task.name" placeholder="Task Name" />
            </td>
            <td v-else>{{ task.name }}</td>
            <td v-if="task.isEditing">
              <select v-model="task.state">
                <option value="not started">Not Started</option>
                <option value="ongoing">Ongoing</option>
                <option value="completed">Completed</option>
              </select>
            </td>
            <td v-else>{{ task.state }}</td>
            <td v-if="task.isEditing">
              <textarea v-model="task.description" placeholder="Description"></textarea>
              <button @click="saveTask(task)">Save</button>
              <button @click="cancelEdit(task)">Cancel</button>
            </td>
            <td v-else>{{ task.description }}</td>
          </tr>
        </tbody>
      </table>
      <div v-if="contextMenuVisible" class="context-menu" :style="{ top: `${contextMenuY}px`, left: `${contextMenuX}px` }" @click.stop>
        <button @click="editTask(contextMenuTask)">Edit</button>
        <button @click="deleteTask(contextMenuTask.id)">Delete</button>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted, onBeforeUnmount } from 'vue';
  
  export default {
    props: ['tasks'],
    emits: ['delete-task', 'edit-task'],
    setup(_, { emit }) {
      const contextMenuVisible = ref(false);
      const contextMenuX = ref(0);
      const contextMenuY = ref(0);
      const contextMenuTask = ref(null);
  
      const showContextMenu = (event, task) => {
        const taskElement = event.target.closest('tr');
        const rect = taskElement.getBoundingClientRect();
        contextMenuX.value = rect.right;
        contextMenuY.value = rect.top;
        contextMenuTask.value = task;
        contextMenuVisible.value = true;
      };
  
      const hideContextMenu = () => {
        contextMenuVisible.value = false;
      };
  
      const deleteTask = (id) => {
        emit('delete-task', id);
        hideContextMenu();
      };
  
      const editTask = (task) => {
        task.isEditing = true;
        hideContextMenu();
      };
  
      const saveTask = (task) => {
        task.isEditing = false;
        emit('edit-task', task);
      };
  
      const cancelEdit = (task) => {
        task.isEditing = false;
      };
  
      const formatDate = (date) => {
        return new Date(date).toLocaleDateString();
      };
  
      const handleClickOutside = (event) => {
        if (!event.target.closest('.context-menu')) {
          hideContextMenu();
        }
      };
  
      onMounted(() => {
        document.addEventListener('click', handleClickOutside);
      });
  
      onBeforeUnmount(() => {
        document.removeEventListener('click', handleClickOutside);
      });
  
      return {
        contextMenuVisible,
        contextMenuX,
        contextMenuY,
        contextMenuTask,
        showContextMenu,
        hideContextMenu,
        deleteTask,
        editTask,
        saveTask,
        cancelEdit,
        formatDate
      };
    }
  };
  </script>
  
  <style>
  .task-list {
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
  }
  table {
    width: 100%;
    border-collapse: collapse;
  }
  th, td {
    border: 1px solid #ccc;
    padding: 10px;
    text-align: left;
  }
  th {
    background-color: #f0f0f0;
  }
  .context-menu {
    position: absolute;
    background-color: white;
    border: 1px solid #ccc;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    z-index: 1000;
  }
     .context-menu button {
       display: block;
       width: 100%;
       padding: 10px;
       border: none;
       background: none;
       text-align: left;
     }
     .context-menu button:hover {
       background-color: #f0f0f0;
     }
     </style>
  
  
  
  
  
  
  
  
  
  
  
  