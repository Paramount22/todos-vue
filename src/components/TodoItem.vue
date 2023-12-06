<script setup>
import { ref } from 'vue';
const props = defineProps({ todo: Object });

const completed = props.todo.completed;
const emit = defineEmits(['chceck-completed', 'delete-todo', 'start-editing']);

const isEdit = ref(false);
const todoText = ref(null);

const checkCompleted = () => {
  props.todo.completed = !props.todo.completed;
  emit('chceck-completed', {
    itemId: props.todo.id,
    completed: props.todo.completed,
  });
};

const startEditTodoText = () => {
  isEdit.value = true;
  /* setTimeout(() => {
    todoText.value.focus();
  }, 0); */
  selectAllText();
};

const saveTodoText = () => {
  isEdit.value = false;
  emit('start-editing', {
    todoId: props.todo.id,
    text: todoText.value.textContent,
  });
};

const resetTodoText = () => {
  isEdit.value = false;
  todoText.value.textContent = props.todo.text;
};

const selectAllText = () => {
  setTimeout(() => {
    let p = todoText.value,
      s = window.getSelection(),
      r = document.createRange();
    r.setStart(p, 0);
    r.setEnd(p, 1);
    s.removeAllRanges();
    s.addRange(r);
    p.focus();
  }, 0);
};
</script>

<template>
  <li class="todo-item" :class="{ opacity: todo.completed === true }">
    <input type="checkbox" v-model="completed" @change="checkCompleted" />
    <p
      class="todo-text"
      ref="todoText"
      :contenteditable="isEdit"
      :class="{ completed: todo.completed === true, border: isEdit === true }"
      @blur="isEdit = false"
      @keydown.enter="saveTodoText"
      @keydown.escape="resetTodoText"
    >
      {{ todo.text }}
    </p>

    <span class="todo-action">
      <span v-if="todo.completed === false" @click="startEditTodoText">
        <i class="fa-regular fa-pen-to-square"></i>
      </span>
      <span @click.prevent="emit('delete-todo', todo.id)">
        <i class="fa-solid fa-trash-can"></i>
      </span>
    </span>
  </li>
</template>

<style lang="scss" scoped>
.border {
  border: 3px solid #2193b0;
}
</style>
