<template>
  <div>
    <h2 :style="{ color: 'yellow', fontSize: fontSize + 'px' }" :class="{ 'slide-animation': isAnimating }">Belajar Mengetik</h2>


    <input
      v-model="textInput"
      placeholder="Masukkan Nama..."
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

    <p v-else :style="{ color: 'yellow' }" class="no-item-msg">Tidak ada Nama yang ditambahkan.</p>

  </div>
</template>

<script setup>
import { ref, defineExpose } from "vue";

const textInput = ref("");
const itemList = ref([]);
const textColor = ref("black");
const fontSize = ref(24);
const isAnimating = ref(false);

defineExpose({ textInput });

const addItem = () => {
  if (textInput.value.trim() !== "") {
    itemList.value.push(textInput.value);
    textInput.value = "";
  }
};

const removeItem = (index) => {
  itemList.value.splice(index, 1);
};
</script>

<style scoped>
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
</style>

