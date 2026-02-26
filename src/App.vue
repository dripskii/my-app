<template>
  <div class="container mt-5">
    <h1 class="text-center mb-4">Моя коллекция книг</h1>

    <!-- Форма для добавления книги -->
    <div class="row">
      <div class="col-md-6 offset-md-3">
        <form @submit.prevent="addBook" class="input-group mb-4">
          <input 
            v-model="newBook.title" 
            type="text" 
            class="form-control" 
            placeholder="Название книги"
            required
          >
          <input 
            v-model="newBook.description" 
            type="text" 
            class="form-control mt-2" 
            placeholder="Описание книги"
            required
          >
          <input 
            v-model="newBook.image" 
            type="text" 
            class="form-control mt-2" 
            placeholder="Ссылка на изображение"
            required
          >
          <button class="btn btn-primary mt-2" type="submit">Добавить книгу</button>
        </form>
      </div>
    </div>

    <!-- Фильтры сортировки -->
    <div class="mb-3">
      <button class="btn btn-secondary" @click="sortBooks('title')">Сортировать по названию</button>
      <button class="btn btn-secondary ms-2" @click="sortBooks('date')">Сортировать по дате добавления</button>
    </div>

    <!-- Отображение коллекции книг -->
    <div class="row">
      <div 
        v-for="book in books" 
        :key="book.id" 
        class="col-md-4 mb-3"
      >
        <div class="card h-100">
          <img :src="book.image" class="card-img-top" alt="book image" />
          <div class="card-body">
            <h5 class="card-title">{{ book.title }}</h5>
            <p class="card-text">{{ book.description }}</p>
            <button 
              @click="removeBook(book.id)" 
              class="btn btn-sm btn-danger"
            >
              Удалить
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Состояния для книг и формы
const books = ref([])
const newBook = ref({ title: '', description: '', image: '' })

// Добавление новой книги в коллекцию
const addBook = () => {
  books.value.push({
    id: Date.now(),
    title: newBook.value.title,
    description: newBook.value.description,
    image: newBook.value.image,
    date: new Date() // добавляем дату для сортировки по дате
  })
  newBook.value = { title: '', description: '', image: '' }
}

// Удаление книги по id
const removeBook = (id) => {
  books.value = books.value.filter(book => book.id !== id)
}

// Функция сортировки книг по названию или дате
const sortBooks = (criteria) => {
  if (criteria === 'title') {
    books.value.sort((a, b) => a.title.localeCompare(b.title))
  } else if (criteria === 'date') {
    books.value.sort((a, b) => b.date - a.date) // сортировка по дате добавления
  }
}
</script>