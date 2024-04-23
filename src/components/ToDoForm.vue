<template>
  <form @submit.prevent="onSubmit">
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
    />
    <button type="submit" class="btn btn__primary btn__lg">Add</button>

    <!-- Tombol filter untuk item yang sudah dicentang -->
    <button type="button" class="btn btn__primary btn__lg" @click="filterChecked">Filter Checked</button>

    <!-- Tombol filter untuk item yang belum dicentang -->
    <button type="button" class="btn btn__primary btn__lg" @click="filterUnchecked">Filter Unchecked</button>
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
