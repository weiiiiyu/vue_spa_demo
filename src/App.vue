<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

//const books = ref([]);

const CollectionUrl = ref('');
const Collections = ref([]);

//const syncCollections = function () {
//  axios.get('https://api.5xcamp.us/api/books/tenlong').then((resp) => {
//    books.value = resp.data.books;
//  });
//};

//const syncCollections = async function () {
//  const resp = await axios.get('https://api.5xcamp.us/api/books/tenlong');
//  books.value = resp.data.books;
//};

onMounted(() => {
  const storage = localStorage.getItem('auo-Collection');

  if (storage != null) {
    const result = JSON.parse(storage);
    Collections.value = result;
  }
});

const clearCollection = () => {
  Collections.value = [];
  localStorage.removeItem('auo-Collection');
};

const addCollection = () => {
  if (CollectionUrl.value != '') {
    const Collection = { id: crypto.randomUUID(), url: CollectionUrl.value };
    Collections.value.unshift(Collection);

    localStorage.setItem('auo-Collection', JSON.stringify(Collections.value));

    CollectionUrl.value = '';
  }
};

const OpenNewTab = (url) => {
  console.log('Opening URL:', url);
  window.open(url, '_blank');
};

const DeleteCollection = (id) => {
  Collections.value = Collections.value.filter(
    (Collection) => Collection.id !== id
  );
  localStorage.setItem('auo-Collection', JSON.stringify(Collections.value));
};
</script>

<template>
  <div>
    <h1 class="display-4 my-3">
      收藏網址⼩⼯具
      <button @click="addCollection" class="btn btn-primary">新增</button>
      <button @click="clearCollection" class="btn btn-danger">清除</button>
    </h1>
    <div class="mb-3">
      <input
        @keyup.enter="addCollection"
        v-model.trim="CollectionUrl"
        type="text"
        class="form-control"
        placeholder="輸入設備名稱"
      />
    </div>
    <hr />
    <ul class="list-group">
      <li
        v-for="Collection in Collections"
        :key="Collection.id"
        class="list-group-item"
      >
        {{ Collection.url }}
        <button @click="OpenNewTab(Collection.url)" class="btn btn-success">
          開啟網址
        </button>
        <button @click="DeleteCollection(Collection.id)" class="btn btn-danger">
          刪除
        </button>
      </li>
    </ul>
  </div>
</template>
