<script setup>
import { ref } from 'vue'

import book1 from './assets/1.png'
import book2 from './assets/2.png'
import book3 from './assets/3.png'
import book4 from './assets/4.png'
import book5 from './assets/5.png'

const books = ref([
  {
    id: 1,
    title: 'Книга Один в поле воин',
    description: 'Про самурая',
    genre: 'Японская культура',
    cover: book1,
    adult: false,
    rating: 5
  },
  {
    id: 2,
    title: 'Книга Два ствола',
    description: 'Голандские приключения',
    genre: 'Роман',
    cover: book2,
    adult: true,
    rating: 4
  },
  {
    id: 3,
    title: 'Книга Три сестры',
    description: 'Русская сказка',
    genre: 'Российская культура',
    cover: book3,
    adult: true,
    rating: 3
  },
  {
    id: 4,
    title: 'Книга Четыре собаки',
    description: 'О житие бытие',
    genre: 'Любовный роман',
    cover: book4,
    adult: true,
    rating: 2
  },
  {
    id: 5,
    title: 'Книга Пять звезд',
    description: 'Для продвижения своего блога в инстаграмме',
    genre: 'Фэнтези',
    cover: book5,
    adult: false,
    rating: 9
  }
])
</script>


#====== main ======#
<template>
  <div class="main">
    <h1>Библиотека книг</h1>

    <div class="books">
      <div class="card" v-for="book in books" :key="book.id">
        <img :src="book.cover" :alt="book.title" />

        <div class="content">
          <h2>{{ book.title }}</h2>
          <p class="genre">{{ book.genre }}</p>
          <p class="description">{{ book.description }}</p>
          <p v-show="book.adult" class="adult">18+</p>
        </div>

        <div class="rating">
          <span v-for="n in 5" :key="n">
            {{ n <= book.rating ? '★' : '☆' }}
          </span>
        </div>

      </div>
    </div>
  </div>
</template>



#====== style ======#
<style scoped>
.main {
  max-width: 2000px;
  margin: 40px auto;
  font-family: system-ui;
}

.books {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 20px;
}

.card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  background: #fff;
}

.card img {
  width: 100%;
  height: 280px;
  object-fit: cover;
  border-radius: 4px;
}

.genre {
  font-weight: bold;
  margin-top: 8px;
}

.description {
  font-size: 14px;
  margin: 8px 0;
}

.adult {
  color: red;
  font-weight: bold;
  min-height: 20px; /* можно добавить для надёжности */
}

.rating {
  margin-top: 8px;
  font-size: 20px;
  color: gold;
}
</style>

