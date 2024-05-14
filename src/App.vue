<template>
  <header class="header">
    <nav class="navbar">
      <ul>
        <li><a href="#" @click="setActiveComponent('posts')">Posts</a></li>
        <li><a href="#" @click="setActiveComponent('todos')">Todos</a></li>
      </ul>
    </nav>
  </header>
  <div class="content">
    <!-- Render Komponen Berdasarkan Menu Aktif -->
    <div v-if="activeComponent === 'todos'">
      <h2 :style="{ color: 'white', fontSize: fontSize + 'px' }" :class="{ 'slide-animation': isAnimating }">Anggota Pemuda Pancasila</h2>
      <input
        v-model="textInput"
        placeholder="Masukkan nama..."
        :style="{ 'font-weight': textInput.length > 10 ? 'bold' : 'normal' }"
        :class="{ 'long-text': textInput.length > 10 }"
      />
      <button @click="addItem" class="btn">Tambah</button>
      <ul v-if="itemList.length > 0" class="item-list">
        <li v-for="(item, index) in itemList" :key="index" class="list-item">
          {{ item }}
          <button @click="removeItem(index)" class="btn btn-remove">Hapus</button>
        </li>
      </ul>
      <p v-else :style="{ color: 'white' }" class="no-item-msg">Tidak ada Nama yang ditambahkan.</p>
    </div>
    <div v-else-if="activeComponent === 'posts'">
      <h2 :style="{ color: 'white', fontSize: fontSize + 'px' }" :class="{ 'slide-animation': isAnimating }">Postingan</h2>
      <select v-model="selectedUser" @change="fetchPosts">
        <option value="">Pilih User</option>
        <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
      </select>
      <ul v-if="posts.length > 0" class="post-list">
        <li v-for="post in posts" :key="post.id" class="post-item">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </li> 
      </ul>
<p v-else :class="['no-posts-msg', { loading: isLoading }, 'white-text']">
{{ isLoading ? 'Memuat postingan...' : 'Tidak ada postingan untuk user ini.' }}
</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const textInput = ref("");
const itemList = ref([]);
const fontSize = ref(24);
const isAnimating = ref(false);

const activeComponent = ref('todos');
const users = ref([]);
const selectedUser = ref('');
const posts = ref([]);
const isLoading = ref(false);

const addItem = () => {
  if (textInput.value.trim() !== "") {
    itemList.value.push(textInput.value);
    textInput.value = "";
  }
};

const removeItem = (index) => {
  itemList.value.splice(index, 1);
};

const setActiveComponent = (component) => {
  activeComponent.value = component;
};

const fetchUsers = async () => {
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/users');
    users.value = response.data;
  } catch (error) {
    console.error('Error fetching users:', error);
  }
};

const fetchPosts = async () => {
  if (selectedUser.value === '') {
    posts.value = [];
    return;
  }
  isLoading.value = true;
  try {
    const response = await axios.get(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`);
    posts.value = response.data;
  } catch (error) {
    console.error('Error fetching posts:', error);
  } finally {
    isLoading.value = false;
  }
};

onMounted(() => {
  fetchUsers();
});
</script>

<style scoped>
.h2{
  color:white;
}
.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  color: white;
  z-index: 1000;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.navbar {
  background-color: #c82333;
  display: flex;
  justify-content: center;
  padding: 10px 0;
}

.navbar ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
  display: flex;
  gap: 20px;
}

.navbar ul li {
  display: inline;
}

.navbar ul li a {
  text-decoration: none;
  color: rgb(255, 255, 255);
  font-size: 20px;
  transition: color 0.3s ease-in-out;
}

.navbar ul li a:hover {
  color: #d5dbe1;
}

.content {
  margin-top: 60px;
  padding: 20px;
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  background-color: #296f24;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

.btn:hover {
  background-color: #0056b3;
}

.long-text {
  font-weight: bold;
}

.item-list {
  margin-top: 20px;
  padding: 0;
}

.list-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 4px;
  background-color: #f4f4f4;
}

.list-item:hover {
  background-color: #e0e0e0;
}

.btn-remove {
  background-color: #dc3545;
}

.btn-remove:hover {
  background-color: #c82333;
}

.no-item-msg {
  font-style: italic;
  color: #888;
}

.post-list {
  margin-top: 20px;
  padding: 0;
  color:white;
}

.post-item {
  margin-bottom: 20px;
}

.no-posts-msg {
  font-style: italic;
  color: #888;
}

.no-posts-msg.loading {
  color: #ffffff;
}
</style>
