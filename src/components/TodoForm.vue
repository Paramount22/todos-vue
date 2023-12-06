<script setup>
import { ref } from 'vue';

const emit = defineEmits(['new-todo']);

const todoText = ref('');

const formSubmitted = () => {
  if (todoText.value === '') return;
  emit('new-todo', todoText.value);
  todoText.value = '';
};
</script>

<template>
  <form class="todo-form" @submit.prevent="formSubmitted">
    <input
      type="text"
      class="todo-input"
      placeholder="Add new todo"
      v-model="todoText"
      autofocus
    />
    <button class="todo-btn">Add</button>
  </form>
</template>

<style lang="scss" scoped></style>
