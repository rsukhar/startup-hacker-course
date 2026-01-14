<template>
  <div class="app">
    <div class="header">
      <h1>Мой рейтинг прочитанных книг</h1>
      <button @click="openAddForm" class="add-button">Добавить книгу</button>
    </div>

    <div v-if="isFormOpen" class="form-overlay" @click.self="closeForm">
      <div class="form-container">
        <h2>{{ editingBookId ? "Редактировать книгу" : "Добавить книгу" }}</h2>

        <form @submit.prevent="submitForm">
          <div class="form-group">
            <label>Название</label>
            <input type="text" v-model="form.title" required />
          </div>

          <div class="form-group">
            <label>Описание</label>
            <textarea v-model="form.description" rows="4"></textarea>
          </div>

          <div class="form-group">
            <label>Обложка (URL)</label>
            <input type="text" v-model="form.cover" />
          </div>

          <div class="form-group">
            <label>Жанр (можно выбрать несколько)</label>
            <select multiple v-model="form.genre" size="4">
              <option v-for="genre in genres" :key="genre" :value="genre">
                {{ genre }}
              </option>
            </select>
          </div>

          <div class="form-group">
            <label>
              <input type="checkbox" v-model="form.isAdult" />
              18+
            </label>
          </div>

          <div class="form-buttons">
            <button type="submit" class="submit-button">
              {{ editingBookId ? "Сохранить" : "Добавить" }}
            </button>
            <button type="button" @click="closeForm" class="cancel-button">
              Отменить
            </button>
          </div>
        </form>
      </div>
    </div>

    <div class="cards">
      <div v-for="book in books" :key="book.id" class="book-card">
        <div class="book-cover-wrapper">
          <img :src="book.cover" :alt="book.title" class="book-cover" />

          <div class="cover-rating-badge">
            <FontAwesomeIcon
              icon="star"
              class="cover-rating-icon"
              :class="{ empty: getCurrentRating(book) === 0 }"
            />
            <span class="cover-rating-text">{{
              getCurrentRating(book) === 0
                ? "-"
                : getCurrentRating(book).toFixed(1)
            }}</span>
          </div>
        </div>

        <div class="book-info">
          <div class="title-wrapper">
            <h2 class="title">{{ book.title }}</h2>
            <span v-if="book.isAdult" class="adult-badge">18+</span>
          </div>
          <p class="genre">{{ book.genre }}</p>

          <div class="rating" :class="{ disabled: book.userRating !== null }">
            <div
              v-for="star in 5"
              :key="star"
              class="star-wrapper"
              @click="setRating(book, star)"
            >
              <FontAwesomeIcon icon="star" class="star star-empty" />
              <FontAwesomeIcon
                icon="star"
                class="star star-filled"
                :style="getStarStyle(star, book.userRating ?? book.rating)"
              />
            </div>
          </div>

          <p class="description">{{ book.description }}</p>

          <div class="book-actions">
            <button @click="openEditForm(book)" class="edit-button">
              Редактировать
            </button>
            <button @click="deleteBook(book.id)" class="delete-button">
              Удалить
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

import martinEden from "./assets/books/martin-eden.jpg";
import flowersForAlgernon from "./assets/books/flowers-for-algernon.jpg";
import alchemist from "./assets/books/alchemist.jpg";
import grokkingAlgorithms from "./assets/books/grokking-algorithms.jpg";
import nightfall from "./assets/books/nightfall.jpg";

const books = ref([
  {
    id: 1,
    title: "Мартин Иден",
    description:
      "Роман о парне из рабочего класса, который преодолевает социальные барьеры, становясь известным писателем. Все это он делает ради любви, но в итоге разочаровывается в том обществе в котором живет та, ради которой он все это делал",
    genre: "Классическая литература",
    cover: martinEden,
    isAdult: false,
    rating: 5,
    userRating: null,
  },
  {
    id: 2,
    title: "Цветы для Элджернона",
    description:
      "История про умственно отсталого уборщика Чарли, который соглашается на экспериментальную операцию по повышению интеллекта.",
    genre: "Научная фантастика",
    cover: flowersForAlgernon,
    isAdult: false,
    rating: 4.5,
    userRating: null,
  },
  {
    id: 3,
    title: "Сумрак",
    description:
      "История студентки, которая влюбляется в своего профессора и вовлекается в запретные отношения",
    genre: "Эротический роман",
    cover: nightfall,
    isAdult: true,
    rating: 4.5,
    userRating: null,
  },
  {
    id: 4,
    title: "Грокаем алгоритмы",
    description:
      "Руководство на Python по основным алгоритмам и структурам данных.",
    genre: "Компьютерная литература",
    cover: grokkingAlgorithms,
    isAdult: false,
    rating: 4,
    userRating: null,
  },
  {
    id: 5,
    title: "Алхимик",
    description:
      "Притча о пастухе Сантьяго, который отправляется в путешествие через пустыню в поисках спрятанного сокровища и постигает истинную судьбу.",
    genre: "Философский роман",
    cover: alchemist,
    isAdult: false,
    rating: 4,
    userRating: null,
  },
]);

const isFormOpen = ref(false);
const editingBookId = ref(null);

const form = ref({
  title: "",
  description: "",
  cover: "",
  genre: [],
  isAdult: false,
});

const genres = [
  "Автобиография",
  "Бизнес",
  "Биография",
  "Детектив",
  "Детская литература",
  "Документальная литература",
  "Драма",
  "Исторический роман",
  "Комедия",
  "Компьютерная литература",
  "Классическая литература",
  "Магический реализм",
  "Мемуары",
  "Научная фантастика",
  "Научпоп",
  "Нон-фикшн",
  "Постмодернизм",
  "Поэзия",
  "Приключения",
  "Психологический роман",
  "Психология",
  "Романтика",
  "Саморазвитие",
  "Современная проза",
  "Триллер",
  "Ужасы",
  "Фэнтези",
  "Философский роман",
  "Хоррор",
  "Эпос",
  "Эротический роман",
  "Юмор",
];

const setRating = (book, rating) => {
  if (book.userRating === null) {
    book.userRating = rating;
  }
};

const getStarStyle = (starNumber, rating) => {
  const fillPercentage = Math.max(
    0,
    Math.min(100, (rating - (starNumber - 1)) * 100)
  );

  if (fillPercentage <= 0) {
    return { clipPath: "inset(100% 0 0 0)" };
  }

  if (fillPercentage >= 100) {
    return { clipPath: "none" };
  }

  return { clipPath: `inset(0 ${100 - fillPercentage}% 0 0)` };
};

const getCurrentRating = (book) => {
  return book.userRating ?? book.rating ?? 0;
};

const openAddForm = () => {
  editingBookId.value = null;
  form.value = {
    title: "",
    description: "",
    cover: "",
    genre: [],
    isAdult: false,
  };

  isFormOpen.value = true;
};

const openEditForm = (book) => {
  editingBookId.value = book.id;
  form.value = {
    title: book.title,
    description: book.description,
    cover: typeof book.cover === "string" ? book.cover : "",
    genre: Array.isArray(book.genre) ? book.genre : [book.genre],
    isAdult: book.isAdult || false,
  };

  isFormOpen.value = true;
};

const closeForm = () => {
  isFormOpen.value = false;
  editingBookId.value = null;
  form.value = {
    title: "",
    description: "",
    cover: "",
    genre: [],
    isAdult: false,
  };
};

const submitForm = () => {
  if (!form.value.title.trim()) return;

  if (editingBookId.value) {
    const book = books.value.find((b) => b.id === editingBookId.value);
    if (book) {
      book.title = form.value.title;
      book.description = form.value.description;
      book.cover = form.value.cover;
      book.genre = form.value.genre.join(", ");
      book.isAdult = form.value.isAdult;
    }
  } else {
    const newBook = {
      id: Date.now(),
      title: form.value.title,
      description: form.value.description,
      cover: form.value.cover,
      genre: form.value.genre.join(", "),
      isAdult: form.value.isAdult,
      rating: 0,
      userRating: null,
    };

    books.value.push(newBook);
  }

  closeForm();
};

const deleteBook = (bookId) => {
  const index = books.value.findIndex((b) => b.id === bookId);
  if (index !== -1) {
    books.value.splice(index, 1);
  }
};
</script>

<style scoped>
:global(html),
:global(body) {
  margin: 0;
  padding: 0;
  height: 100%;
}

:global(body) {
  background: #d8d8cc;
}

.app {
  min-height: 100vh;
  padding: 0 0 50px 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto",
    "Helvetica Neue", Arial, sans-serif;
}

.header {
  border-radius: 50px;
  padding: 0.75rem 1rem;
  margin: 40px auto 40px auto;
  width: 100%;
  max-width: 960px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.08);
  border: 2px solid #5f7367;
  background: #d8d8cc;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;
}

.header h1 {
  font-size: 1.9rem;
  color: #5f7367;
  font-weight: 600;
  letter-spacing: 0.3px;
  margin: 0;
}

.add-button {
  padding: 0.6rem 1.5rem;
  border: none;
  background: rgba(95, 115, 103, 0.9);
  color: #ffffff;
  font-size: 1rem;
  cursor: pointer;
  font-weight: 600;
  border-radius: 15px;
  transition: all 0.2s ease;
}

.add-button:hover {
  background: rgba(95, 115, 103, 1);
}

.cards {
  max-width: 1200px;
  margin: 0 auto 50px auto;
  padding: 0 2rem;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
  align-items: start;
}

.book-card {
  background-color: rgba(95, 115, 103, 0.4);
  border-radius: 20px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  box-shadow: 0 4px 12px rgba(57, 73, 63, 0.25);
  border: 1px solid #5f7367;
  transition: all 0.3s ease;
  cursor: pointer;
  position: relative;
  z-index: 1;
  box-sizing: border-box;
}

.book-card:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 26px rgba(57, 73, 63, 0.35);
  background-color: rgba(95, 115, 103, 1);
  z-index: 10;
}

.book-cover-wrapper {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  margin-bottom: 8px;
}

.book-cover {
  width: 100%;
  max-width: 220px;
  height: 320px;
  object-fit: cover;
  border-radius: 14px;
  box-shadow: 0 4px 10px rgba(15, 23, 42, 0.25);
}

.cover-rating-badge {
  position: absolute;
  top: 10px;
  right: 20px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.cover-rating-icon {
  font-size: 32px;
  color: #fbbf24;
  filter: drop-shadow(0 2px 4px rgba(15, 23, 42, 0.5));
}

.cover-rating-icon.empty {
  color: #9ca3af;
}

.cover-rating-text {
  position: absolute;
  font-size: 10px;
  font-weight: 600;
  color: #141a26;
}

.book-info {
  display: flex;
  flex-direction: column;
  flex: 1;
  gap: 8px;
}

.title-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  flex-wrap: wrap;
}

.title {
  margin: 0;
  font-size: 20px;
  font-weight: 600;
  color: #fdfdf9;
  line-height: 1.3;
  text-align: center;
}

.adult-badge {
  display: inline-block;
  padding: 2px 8px;
  background-color: #e11d48;
  color: #fff;
  font-size: 12px;
  font-weight: 600;
  border-radius: 999px;
  line-height: 1.4;
}

.genre {
  margin: 0;
  font-size: 14px;
  color: #e2e8f0;
  font-style: italic;
  text-align: center;
}

.rating {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 4px;
  margin: 4px 0;
}

.rating.disabled {
  pointer-events: none;
  opacity: 0.7;
}

.star-wrapper {
  position: relative;
  display: inline-block;
}

.star {
  font-size: 18px;
  transition: color 0.2s ease;
}

.star-empty {
  color: #e5e7eb;
}

.star-filled {
  position: absolute;
  top: 0;
  left: 0;
  color: #fbbf24;
}

.description {
  margin: 0;
  font-size: 14px;
  color: #f9fafb;
  line-height: 1.6;
  opacity: 0;
  max-height: 0;
  overflow: hidden;
  transition: opacity 0.3s ease, max-height 0.3s ease, margin 0.3s ease;
}

.book-card:hover .description {
  opacity: 1;
  max-height: 200px;
  margin-top: 8px;
}

.book-actions {
  display: flex;
  gap: 6px;
  margin-top: 8px;
  width: 100%;
}

.edit-button,
.delete-button {
  flex: 0 0 calc(50% - 3px);
  padding: 0.45rem 0.6rem;
  border: none;
  background: rgba(255, 255, 255, 0.2);
  color: #ffffff;
  font-size: 0.9rem;
  font-weight: 500;
  cursor: pointer;
  border-radius: 14px;
  transition: all 0.2s ease;
  box-sizing: border-box;
  text-align: center;
}

.edit-button:hover,
.delete-button:hover {
  background: rgba(255, 255, 255, 0.3);
}

.form-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10000;
  backdrop-filter: blur(4px);
}

.form-container {
  background: #ffffff;
  border-radius: 30px;
  padding: 2rem;
  max-width: 400px;
  width: 90%;
  position: relative;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
}

.form-container h2 {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 1.5rem;
  color: #333;
  text-align: center;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.form-group label {
  font-size: 0.9rem;
  font-weight: 500;
  color: #333;
}

.form-group input[type="text"],
.form-group textarea {
  padding: 0.75rem 1rem;
  border: 2px solid #e0e0e0;
  border-radius: 15px;
  font-size: 0.95rem;
  transition: all 0.2s;
  background: #ffffff;
  color: #333;
  font-family: inherit;
}

.form-group input[type="text"]:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #5f7367;
}

.form-group textarea {
  resize: vertical;
}

.form-group select {
  padding: 0.75rem 1rem;
  border: 2px solid #e0e0e0;
  border-radius: 15px;
  font-size: 0.95rem;
  transition: all 0.2s;
  background: #ffffff;
  color: #333;
}

.form-group select:focus {
  outline: none;
  border-color: #5f7367;
}

.form-group input[type="checkbox"] {
  margin-right: 8px;
}

.form-buttons {
  display: flex;
  gap: 1rem;
  margin-top: 0.5rem;
}

.submit-button,
.cancel-button {
  flex: 1;
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 15px;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
}

.submit-button {
  background: #5f7367;
  color: #ffffff;
}

.submit-button:hover {
  background: #4a5a52;
  transform: scale(1.02);
}

.cancel-button {
  background: #e0e0e0;
  color: #333;
}

.cancel-button:hover {
  background: #d0d0d0;
}
</style>
