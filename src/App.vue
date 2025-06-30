<script setup>
import { ref, onMounted } from 'vue';

const bookName = ref('');
const bookLink = ref('');
const bookmarks = ref([]);

//const syncDevices = function () {
//axios.get('https://api.5xcamp.us/api/books/tenlong').then((resp) => {
//books.value = resp.data.books;
//});
//};
onMounted(() => {
  const storage = localStorage.getItem('auo-books');
  if (storage != null) {
    const result = JSON.parse(storage);
    bookmarks.value = result;
  }
});
const removeBookMark = (a) => {
  var items = JSON.parse(localStorage['auo-books']);
  for (var i = 0; i < items.length; i++) {
    if (items[i].id == a) {
      items.splice(i, 1);
      break;
    }
  }
  localStorage.setItem('auo-books', JSON.stringify(items));
  bookmarks.value = JSON.parse(localStorage.getItem('auo-books'));
};
const clearBookMark = () => {
  bookmarks.value = [];
  localStorage.removeItem('auo-books');
};
const addBookMark = () => {
  if (bookName.value != '' && bookLink.value != '') {
    const book = {
      id: crypto.randomUUID(),
      title: bookName.value,
      link: bookLink.value,
    };
    bookmarks.value.unshift(book);
    localStorage.setItem('auo-books', JSON.stringify(bookmarks.value));
    bookName.value = '';
    bookLink.value = '';
  }
};
</script>

<template>
  <h1 class="text-3xl">
    收藏網址小工具
    <button @click="clearBookMark" class="btn btn-error btn-sm">
      清除全部
    </button>
  </h1>
  標題:
  <input v-model.trim="bookName" type="text" class="input" />
  <br />
  網址:
  <input v-model.trim="bookLink" type="text" class="input" />
  <button @click="addBookMark" class="btn">新增</button>
  <hr />
  <ul>
    <li v-for="book in bookmarks">
      <a href="{{ book.link }}" target="_blank">{{ book.title }}</a>
      <button @click="removeBookMark(book.id)" class="btn">刪除</button>
    </li>
  </ul>
</template>
