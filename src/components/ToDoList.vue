<template>
  <div>
    <h1>To-Do List</h1>
    <to-do-form @todo-added="addToDo" @filter="filterToDo"></to-do-form>
    <h2 id="list-summary" ref="listSummary" tabindex="-1">{{ listSummary }}</h2>
    <ul aria-labelledby="list-summary" class="stack-large" style="background: linear-gradient(to right, #ff9a9e, #fad0c4);">
      <li v-for="item in filteredToDoItems" :key="item.id">
        <slot name="todo-item" :item="item"></slot>
      </li>
    </ul>
  </div>
</template>

<script>
import ToDoForm from "./ToDoForm.vue"; // Assuming ToDoForm component file path
import uniqueId from "lodash.uniqueid";

export default {
  name: "TodoList",
  components: {
    ToDoForm,
  },
  props: {
    todoItems: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      filterType: null,
    };
  },
  methods: {
    addToDo(toDoLabel) {
      this.$emit("todo-added", toDoLabel);
    },
    // Other methods...
  },
  computed: {
    listSummary() {
      const numberFinishedItems = this.todoItems.filter(
        (item) => item.done
      ).length;
      return `${numberFinishedItems} out of ${this.todoItems.length} items completed`;
    },
    filteredToDoItems() {
      if (!this.filterType) {
        return this.todoItems;
      } else if (this.filterType === "checked") {
        return this.todoItems.filter((item) => item.done);
      } else if (this.filterType === "unchecked") {
        return this.todoItems.filter((item) => !item.done);
      }
      return this.todoItems;
    },
  },
};
</script>

<style scoped>
.stack-large {
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
</style>
