<template>
  <div class="app">
    <h1>Мой рейтинг прочитанных книг</h1>

    <div class="cards">
      <div v-for="book in books" :key="book.id" class="book-card">
        <div class="book-cover-wrapper">
          <img :src="book.cover" :alt="book.title" class="book-cover" />
        </div>

        <div class="book-info">
          <div class="title-wrapper">
            <h2 class="title">{{ book.title }}</h2>
            <span v-if="book.isAdult" class="adult-badge">18+</span>
          </div>
          <p class="genre">{{ book.genre }}</p>

          <div class="rating">
            <div v-for="star in 5" :key="star" class="star-wrapper">
              <FontAwesomeIcon icon="star" class="star star-empty" />
              <FontAwesomeIcon
                icon="star"
                class="star star-filled"
                :style="getStarStyle(star, book.rating)"
              />
            </div>
          </div>

          <p class="description">{{ book.description }}</p>
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
  },
]);

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
</script>

<style scoped>
.app {
  min-height: 100vh;
  padding: 32px 16px;
  background: linear-gradient(135deg, #e5f0ff 0%, #f3f4f6 100%);
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
    sans-serif;
}

h1 {
  text-align: center;
  margin-bottom: 32px;
  font-size: 32px;
  color: #1f2937;
  font-weight: 600;
}

.cards {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
  align-items: start;
}

.book-card {
  background-color: #fff;
  border-radius: 16px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  box-shadow: 0 4px 12px rgba(15, 23, 42, 0.08);
  border: 1px solid rgba(148, 163, 184, 0.3);
  transition: all 0.3s ease;
  cursor: pointer;
  position: relative;
  z-index: 1;
}

.book-card:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 24px rgba(15, 23, 42, 0.15);
  background-color: rgba(148, 163, 184, 0.3);
  z-index: 10;
}

.book-cover-wrapper {
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
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(15, 23, 42, 0.2);
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
  color: #111827;
  line-height: 1.3;
  text-align: center;
}

.adult-badge {
  display: inline-block;
  padding: 2px 8px;
  background-color: #ef4444;
  color: #fff;
  font-size: 12px;
  font-weight: 600;
  border-radius: 4px;
  line-height: 1.4;
}

.genre {
  margin: 0;
  font-size: 14px;
  color: #6b7280;
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
  color: #374151;
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
</style>
