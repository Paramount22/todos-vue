<script setup>
const props = defineProps({
  flashAddNewTodo: Boolean,
  flashUpdateTodo: Boolean,
  flashDeleteTodo: Boolean,
});

const todoAdded = 'New todo item added.';
const todoUpdated = 'Todo item was updated.';
const todoDeleted = 'Todo item was deleted.';
</script>

<template>
  <TransitionGroup name="bounce">
    <div v-if="flashAddNewTodo" class="flash">{{ todoAdded }}</div>
    <div v-if="flashUpdateTodo" class="flash">{{ todoUpdated }}</div>
    <div v-if="flashDeleteTodo" class="flash">{{ todoDeleted }}</div>
  </TransitionGroup>
</template>

<style lang="scss" scoped>
.flash {
  position: fixed;
  bottom: 2rem;
  right: 2rem;
  padding-left: 2rem;
  border: 1px solid #198754;
  border-radius: 0.5rem;
  min-width: 20rem;
  height: 3rem;
  background-color: #20c997;
  color: white;
  font-size: 1.5rem;
  display: flex;
  justify-content: flex-start;
  align-items: center;
}

.bounce-enter-active {
  animation: bounce-in 0.3s;
}
.bounce-leave-active {
  animation: bounce-in 0.3s reverse;
}
</style>
