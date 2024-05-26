<!-- app.vue -->
<template>
  <div>
    <!-- Header dengan menu "Post" dan "Todos" -->
    <header>
      <nav>
        <ul>
          <li @click="toggleView('todos')">Todos</li>
          <li @click="toggleView('posts')">Post</li>
        </ul>
      </nav>
    </header>

    <!-- Menampilkan komponen berdasarkan pilihan -->
    <div v-if="view === 'todos'">
      <Todos :initial-activities="initialActivities" @sendData="handleTodosData">
        <template #header>
          <h1>Daftar Kegiatan Kustom</h1>
        </template>
      </Todos>
    </div>

    <div v-else-if="view === 'posts'">
      <Post :custom-users="users" @sendData="handlePostsData">
        <template #header>
          <h1>Postingan Pengguna Kustom</h1>
        </template>
      </Post>
    </div>

    <!-- Tempat menampilkan data yang diterima -->
    <div v-if="receivedData">
      <h2>Data yang Diterima:</h2>
      <pre>{{ receivedData }}</pre>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import Todos from './components/todos.vue';
import Post from './components/post.vue';

const view = ref('todos');
const users = ref([]);
const initialActivities = ref([
  { id: 1, name: 'Makan siang', completed: false },
  { id: 2, name: 'Belajar VueJS', completed: false }
]);

const receivedData = ref(null);

function toggleView(selectedView) {
  view.value = selectedView;
  receivedData.value = null;  // Reset data saat berganti view
}

async function fetchUsers() {
  const response = await fetch('https://jsonplaceholder.typicode.com/users');
  users.value = await response.json();
}

function handleTodosData(data) {
  receivedData.value = data;
}

function handlePostsData(data) {
  receivedData.value = data;
}

onMounted(fetchUsers);
</script>

<style scoped>
/* CSS untuk header */
header {
  background-color: #b1d100; 
  color: white;
  padding: 10px;
}

nav ul {
  list-style-type: none;
  padding: 0;
}

nav ul li {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease; 
}

/* Efek hover */
nav ul li:hover {
  background-color: #8aa000; 
}
</style>
