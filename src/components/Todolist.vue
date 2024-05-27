
<template>
  <div>
    <h1>To-Do List</h1>
    <to-do-form @todo-added="addToDo" @filter="filterToDo"></to-do-form>
    <h2 id="list-summary" ref="listSummary" tabindex="-1">{{ listSummary }}</h2>
    <ul aria-labelledby="list-summary" class="stack-large">
      <li v-for="item in filteredToDoItems" :key="item.id">
        <to-do-item
          :label="item.label"
          :done="item.done"
          :id="item.id"
          @checkbox-changed="updateDoneStatus(item.id)"
          @item-deleted="deleteToDo(item.id)"
          @item-edited="editToDo(item.id, $event)"
        ></to-do-item>
      </li>
    </ul>
  </div>
</template>

<script>
import ToDoItem from "./components/ToDoItem.vue";
import ToDoForm from "./components/ToDoForm.vue";
import uniqueId from "lodash.uniqueid";

export default {
  name: "TodoList",
  components: {
    ToDoItem,
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
    updateDoneStatus(toDoId) {
      this.$emit("update-done-status", toDoId);
    },
    deleteToDo(toDoId) {
      this.$emit("delete-todo", toDoId);
    },
    editToDo(toDoId, newLabel) {
      this.$emit("edit-todo", toDoId, newLabel);
    },
    filterToDo(type) {
      this.filterType = type;
      this.$emit("filter", type);
    },
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
