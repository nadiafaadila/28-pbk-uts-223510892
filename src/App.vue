<!-- App.vue -->
<template>
  <div id="app">
    <div class="header">
      <ul>
        <li @click="selectedMenu = 'Todos'" :class="{ active: selectedMenu === 'Todos' }">Todos</li>
        <li @click="selectedMenu = 'Post'" :class="{ active: selectedMenu === 'Post' }">Post</li>
      </ul>
    </div>
    <div class="container">
      <todo-list v-if="selectedMenu === 'Todos'" :todo-items="ToDoItems" @todo-added="addToDo" @update-done-status="updateDoneStatus" @delete-todo="deleteToDo" @edit-todo="editToDo" @filter="filterToDo"></todo-list>
      <post-list v-if="selectedMenu === 'Post'" :users="users" :user-posts="userPosts"></post-list>
    </div>
  </div>
</template>

<script>
import ToDoList from "./components/TodoList.vue";
import PostList from "./components/PostList.vue";
import uniqueId from "lodash.uniqueid";

export default {
  name: "App",
  components: {
    ToDoList,
    PostList,
  },
  data() {
    return {
      selectedMenu: 'Todos', // Default menu selection
      ToDoItems: [],
      users: [], // Placeholder for users data
      userPosts: [] // Placeholder for user posts data
    };
  },
  methods: {
    addToDo(toDoLabel) {
      this.ToDoItems.push({
        id: uniqueId("todo-"),
        label: toDoLabel,
        done: false,
      });
    },
    updateDoneStatus(toDoId) {
      const toDoToUpdate = this.ToDoItems.find((item) => item.id === toDoId);
      toDoToUpdate.done = !toDoToUpdate.done;
    },
    deleteToDo(toDoId) {
      const itemIndex = this.ToDoItems.findIndex((item) => item.id === toDoId);
      this.ToDoItems.splice(itemIndex, 1);
    },
    editToDo(toDoId, newLabel) {
      const toDoToEdit = this.ToDoItems.find((item) => item.id === toDoId);
      toDoToEdit.label = newLabel;
    },
    filterToDo(type) {
      // Handle filtering
    },
    fetchUsers() {
      fetch('https://jsonplaceholder.typicode.com
