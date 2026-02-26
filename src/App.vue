<template>
  <div class="container mt-5">
    <h1 class="text-center mb-4">Моя коллекция книг</h1>

    <!-- Форма для добавления книги -->
    <div class="row">
      <div class="col-md-4">
        <input 
          v-model="newBook.title" 
          type="text" 
          class="form-control" 
          placeholder="Название книги"
          required
        >
      </div>
      <div class="col-md-4">
        <input 
          v-model="newBook.description" 
          type="text" 
          class="form-control" 
          placeholder="Описание книги"
          required
        >
      </div>
      <div class="col-md-4">
        <input 
          v-model="newBook.image" 
          type="text" 
          class="form-control" 
          placeholder="Ссылка на изображение"
          required
        >
      </div>
    </div>

    <!-- Кнопка "Добавить книгу" теперь в форме -->
    <form @submit.prevent="addBook" class="row mt-3">
      <div class="col-md-4">
        <button class="btn btn-primary w-100" type="submit">Добавить книгу</button>
      </div>
    </form>

    <!-- Фильтры сортировки -->
    <div class="row mt-3">
      <div class="col-md-6">
        <button class="btn btn-secondary w-100" @click="sortBooks('title')">Сортировать по названию</button>
      </div>
      <div class="col-md-6">
        <button class="btn btn-secondary w-100" @click="sortBooks('date')">Сортировать по дате добавления</button>
      </div>
    </div>

    <!-- Отображение коллекции книг -->
    <div class="row mt-4">
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