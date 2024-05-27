<template>
  <form @submit.prevent="onSubmit" class="todo-form">
    <h2 class="label-wrapper">
      <label for="new-todo-input" class="label__lg">
        What needs to be done?
      </label>
    </h2>
    <input
      type="text"
      id="new-todo-input"
      name="new-todo"
      autocomplete="off"
      v-model.lazy.trim="label"
      class="input__lg"
      style="background: linear-gradient(to right, #ff9a9e, #fad0c4);"
    />
    <button type="submit" class="btn btn__primary btn__lg" style="background: linear-gradient(to right, #ff9a9e, #fad0c4);">Add</button>

    <!-- Tombol filter untuk item yang sudah dicentang -->
    <button type="button" class="btn btn__primary btn__lg" @click="filterChecked" style="background: linear-gradient(to right, #ff9a9e, #fad0c4);">Filter Checked</button>

    <!-- Tombol filter untuk item yang belum dicentang -->
    <button type="button" class="btn btn__primary btn__lg" @click="filterUnchecked" style="background: linear-gradient(to right, #ff9a9e, #fad0c4);">Filter Unchecked</button>
  </form>
</template>

<script>
export default {
  methods: {
    onSubmit() {
      if (this.label === "") {
        return;
      }
      this.$emit("todo-added", this.label);
      this.label = "";
    },
    filterChecked() {
      this.$emit("filter", "checked"); // Emit event filter dengan parameter 'checked'
    },
    filterUnchecked() {
      this.$emit("filter", "unchecked"); // Emit event filter dengan parameter 'unchecked'
    }
  },
  data() {
    return {
      label: "",
    };
  },
};
</script>

<style scoped>
.todo-form {
  background: linear-gradient(to right, #ff9a9e, #fad0c4);
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.input__lg {
  width: calc(100% - 80px);
  margin-bottom: 10px;
  padding: 10px;
  border: none;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  font-size: 16px;
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  color: white;
  cursor: pointer;
  transition: background 0.3s ease;
}

.btn:hover {
  opacity: 0.9;
}
</style>
