<!-- PostList.vue -->
<template>
  <div>
    <label for="userSelect">Select User:</label>
    <select id="userSelect" v-model="selectedUser">
      <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
    </select>
    <ul v-if="selectedUser">
      <li v-for="post in userPosts" :key="post.id">
        {{ post.title }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "PostList",
  props: {
    users: {
      type: Array,
      required: true,
    },
    userPosts: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      selectedUser: null,
    };
  },
  watch: {
    selectedUser(newVal) {
      if (newVal) {
        this.fetchUserPosts(newVal);
      }
    },
  },
  methods: {
    fetchUserPosts(userId) {
      fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`)
        .then((response) => response.json())
        .then((data) => (this.userPosts = data))
        .catch((error) =>
          console.error(`Error fetching posts for user ${userId}:`, error)
        );
    },
  },
};
</script>
