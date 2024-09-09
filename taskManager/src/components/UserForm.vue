
<template>
    <div id="userform">
    <form @submit.prevent="handleSubmit">
        <input v-model="name" placeholder="Enter your name" required />
        <button type="submit">Submit</button>
    </form>
    </div>
</template>


<script>
import { ref } from 'vue';

export default {
  emits: ['set-user'],
  setup(_, { emit }) {
    const name = ref('');

    const handleSubmit = () => {
      emit('set-user', name.value);
      name.value = ''; // Reset the input field
    };

    return { name, handleSubmit };
  }
};
</script>

<style src="./src/style.css"></style>

