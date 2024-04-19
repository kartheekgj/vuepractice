<template>
  <div class="about">
    <h1>ToDo List Page</h1>
    <div>
      <input
        v-focus
        type="text"
        class="todo-input"
        @keyup.enter="addTodo"
        placeholder="Todo list data goes here"
        v-model="newTodo"
      />
      <div v-for="(todo, index) in todos" :key="todo.id" class="todo-items">
        <div class="todo-item-left">
          <div class="todo-item-label" v-if="!todo.editing" @dblclick="editTodo(todo)">
            {{ todo.title }}
          </div>
          <input
            v-else
            @blur="doneEdit(todo)"
            @keyup="doneEdit(todo)"
            type="text"
            class="todo-item-edit"
            v-model="todo.title"
          />
        </div>

        <div class="remove-item" @click="removeTodo(index)">&times;</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const newTodo = ref('')
let idForTodo = ref(0)
const todos = ref([]);


const vFocus = {
  mounted: (e) => e.focus()
}

const addTodo = () => {
  if(newTodo.value.trim().length === 0) return
  todos.value.push({
    id: idForTodo.value++,
    title: newTodo.value,
    completed: false,
    editing: false
  });
  newTodo.value = '';
}

const removeTodo = (index) => todos.value.splice(index, 1)
const editTodo = (todo) => (todo.value.editing = true)
const doneEdit = (todo) => (todo.value.editing = false)
</script>

<style>
.todo-input {
  width: 100%;
  padding: 10px 18px;
  margin-bottom: 16px;
}
.todo-items {
  font-size: 24px;
  color: blueviolet;
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.remove-item {
  margin-left: 14px;
  cursor: pointer;
}
.remove-item:hover {
  margin-left: 14px;
  cursor: pointer;
  color: black;
}
.todo-item-edit {
  margin-left: 12px;
  color: aqua;
  background-color: burlywood;
  font-size: 24px;
  border: 1px solid #ccc;
  padding: 10px;
  width: 100%;
}
.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}
.todo-item-left {
  display: flex;
  align-items: center;
}
</style>
