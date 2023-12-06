<script setup>
import TodoItem from '../components/TodoItem.vue';

const props = defineProps({ todos: Array });
const emit = defineEmits(['chceck-completed', 'delete-todo', 'edit-todo-text']);

/* const log = (data) => {
  console.log(data);
}; */
</script>

<template>
  <TransitionGroup tag="ul" name="bounce" class="todo-list">
    <TodoItem
      v-for="todo in todos"
      :key="todo.id"
      :todo="todo"
      @chceck-completed="emit('chceck-completed', $event)"
      @delete-todo="emit('delete-todo', $event)"
      @start-editing="emit('edit-todo-text', $event)"
    />
  </TransitionGroup>
</template>

<style lang="scss" scoped>
.bounce-enter-active {
  animation: bounce-in 0.5s;
}
.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
</style>
