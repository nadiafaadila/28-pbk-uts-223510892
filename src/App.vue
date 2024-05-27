<template>
  <div id="app">
    <div class="header">
      <ul>
        <li @click="selectedMenu = 'Todos'" :class="{ active: selectedMenu === 'Todos' }">Todos</li>
        <li @click="selectedMenu = 'Post'" :class="{ active: selectedMenu === 'Post' }">Post</li>
      </ul>
    </div>
    <div class="container">
      <todo-list v-if="selectedMenu === 'Todos'" :todo-items="ToDoItems">
        <template v-slot:todo-item="props">
          <to-do-item
            :label="props.item.label"
            :done="props.item.done"
            :id="props.item.id"
            @checkbox-changed="updateDoneStatus(props.item.id)"
            @item-deleted="deleteToDo(props.item.id)"
            @item-edited="editToDo(props.item.id, $event)"
          ></to-do-item>
        </template>
      </todo-list>
      <post-list v-if="selectedMenu === 'Post'" :users="users" :user-posts="userPosts">
        <template v-slot:post-item="props">
          <li :key="props.post.id">
            {{ props.post.title }}
          </li>
        </template>
      </post-list>
    </div>
  </div>
</template>

<script>
import ToDoList from "./components/TodoList.vue";
import PostList from "./components/PostList.vue";

export default {
  name: "App",
  components: {
    ToDoList,
    PostList,
  },
  // rest of the code...
};
</script>
