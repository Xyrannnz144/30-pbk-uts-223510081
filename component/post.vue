<template>
  <div>
    <h1>Posts</h1>
    <select v-model="selectedUser">
      <option v-for="user in users" :key="user.id" :value="user.id">
        {{ user.name }}
      </option>
    </select>
    <ul>
      <li v-for="post in filteredPosts" :key="post.id">
        {{ post.title }}
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import axios from 'axios'

export default {
  props: {
    users: {
      type: Array,
      required: true
    }
  },
  setup(props) {
    const selectedUser = ref(1)

    const posts = ref([])

    const fetchPosts = async () => {
      const response = await axios.get(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`)
      posts.value = response.data
    }

    const filteredPosts = computed(() => {
      return posts.value.filter(post => post.userId === selectedUser.value)
    })

    fetchPosts()

    return { selectedUser, filteredPosts }
  }
}
</script>

<style>
/* Add some styles */
</style>