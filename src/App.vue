<script setup>
import TodoForm from './components/TodoForm.vue';
import TodoList from './components/TodoList.vue';
import TodoFlash from './components/TodoFlash.vue';
import { useStorage } from '@vueuse/core';
import { nanoid } from 'nanoid';
import { data } from './data';
import { ref } from 'vue';

const todos = useStorage('todos', data);
const flashAddNewTodo = ref(false);
const flashUpdateTodo = ref(false);
const flashDeleteTodo = ref(false);

console.log(todos.value.length);

const addNewTodo = (todo) => {
  flashAddNewTodo.value = true;
  const newTodo = {
    id: nanoid(),
    text: todo,
    completed: false,
  };

  todos.value.push(newTodo);
  setTimeout(() => {
    flashAddNewTodo.value = false;
  }, 2000);
};

const completedTodo = (data) => {
  if (!data?.id || !data?.completed) return;
  todos.value.filter((todo) => {
    if (todo.id === data.id) {
      todo.completed = data.completed;
    }
  });
};

const deleteTodo = (id) => {
  if (!id) return;
  flashDeleteTodo.value = true;
  todos.value = todos.value.filter((todo) => todo.id !== id);
  setTimeout(() => {
    flashDeleteTodo.value = false;
  }, 2000);
};

const editTodoText = (data) => {
  if (!data?.todoId || !data?.text) return;
  flashUpdateTodo.value = true;
  todos.value.find((todo) => {
    if (todo.id === data.todoId) {
      todo.text = data.text;
    }
  });
  setTimeout(() => {
    flashUpdateTodo.value = false;
  }, 2000);
};
</script>

<template>
  <TodoFlash
    :flashAddNewTodo="flashAddNewTodo"
    :flashUpdateTodo="flashUpdateTodo"
    :flashDeleteTodo="flashDeleteTodo"
  />

  <header class="todo-header">
    <div class="todo-title">
      <h1>Todo list</h1>
      <i class="fa-solid fa-list"></i>
    </div>
  </header>
  <main>
    <TodoForm @new-todo="addNewTodo($event)" />

    <div class="todo-items">
      <h2 v-if="todos.length === 0">No items</h2>
    </div>

    <TodoList
      :todos="todos"
      @chceck-completed="completedTodo($event)"
      @delete-todo="deleteTodo($event)"
      @edit-todo-text="editTodoText"
    />
  </main>
</template>

<style scoped></style>
