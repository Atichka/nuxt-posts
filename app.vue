<template>
  <div>
    <table class="table main">
      <thead class="table__header">
        <tr>
          <th class="table__header_row" style="width: 10%;">ID</th>
          <th class="table__header_row" style="width: 30%;">Title</th>
          <th class="table__header_row" style="width: 60%;">Body</th>
        </tr>
      </thead>
      <tbody class="table__body divide-y divide-gray-200">
        <tr v-for="post in posts" :key="post.id">
          <td class=" table__body_row">{{ post.id }}</td>
          <td class=" table__body_row">{{ post.title }}</td>
          <td class=" table__body_row">{{ post.body }}</td>
        </tr>
      </tbody>
    </table>
    <div class="buttons flex justify-between mt-4">
      <button @click="previousPage" :disabled="currentPage <= 1" class="button">Previous</button>
      <button @click="nextPage" :disabled="currentPage >= totalPages" class="button">Next</button>
    </div>
  </div>
</template>
<script setup>
import {ref, computed} from 'vue'
import axios from "axios";
const currentPage = ref(1)
const postsPerPage = 10
const posts = ref([])

const columns = [{
  key: 'userId',
  label: 'UserID'
}, {
  key: 'id',
  label: 'Id'
}, {
  key: 'title',
  label: 'Title'
}, {
  key: 'body',
  label: 'Body'
}]

const fetchPosts = async () => {
  try {
    const response = await axios.get(`https://jsonplaceholder.typicode.com/posts?_page=${currentPage.value}&_limit=${postsPerPage}`)
    posts.value = response.data
  } catch (error) {
    console.error(error)
  }
}

const totalPosts = ref(100) // всего количество постов JSONPlaceholder
const totalPages = computed(() => Math.ceil(totalPosts.value / postsPerPage))

if (currentPage.value < totalPages.value) {
  currentPage.value++
  fetchPosts()
}

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++
    fetchPosts()
  }
}

const previousPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--
    fetchPosts()
  }
}

fetchPosts()

</script>

<style>
.table {
  text-indent: 0;
  border-color: inherit;
  border-collapse: collapse;
}
.table__header {
  background-color: grey;
}
.table__header_row {
  padding: 0.75rem 1.5rem;
  text-align: left;
  font-size: 0.75rem;
  line-height: 1rem;
  font-weight: 500;
  --tw-text-opacity: 1;
  color: rgba(248, 247, 247, 0.95);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}
.table__body {
  background-color: #bebebe;
}
.table__body_row {
  padding: 1rem 1.5rem;
}
.buttons {
  justify-content: space-between;
  display: flex;
  margin-top: 1rem;
}
.button {
  width: 10%;
  color: white;
  padding: 0.5rem 1rem;
  background-color: grey;
  border-radius: 0.375rem;
  cursor: pointer;
}
</style>
