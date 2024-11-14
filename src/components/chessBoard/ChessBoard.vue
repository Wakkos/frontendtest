<template>
  <div class="playpage">
    <!-- Board -->
    <main class="playpage__board">
      <div class="board-grid">
        <div v-for="rank in ranks" :key="rank" class="rank">
          <div
            v-for="file in files"
            :key="`${file}${rank}`"
            class="board-grid__square"
            :class="{
              whites: isLightSquare(file, rank),
              blacks: !isLightSquare(file, rank),
              clicked: isClicked(`${file}${rank}`)
            }"
            @click="handleSquareClick(`${file}${rank}`)"
          >
            <span class="board-grid__square__notation">{{ `${file}${rank}` }}</span>
          </div>
        </div>
      </div>
    </main>
    <!-- Aside -->
    <div class="playpage__notes">
      <h2 class="playpage__notes__title">Clicked</h2>
      <div v-if="clickedSquares.length === 0" class="playpage__notes__empty">No squares clicked yet</div>
      <ol v-else class="playpage__clicks">
        <li v-for="(square, index) in clickedSquares" :key="index" class="playpage__clicks__item">
          {{ square }}
        </li>
      </ol>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const files = ["a", "b", "c", "d", "e", "f", "g", "h"];
const ranks = ["1", "2", "3", "4", "5", "6", "7", "8"]; // De abajo hacia arriba
const clickedSquares = ref([]);

const isLightSquare = (file, rank) => {
  return (files.indexOf(file) + ranks.indexOf(rank)) % 2 === 0;
};

const isClicked = (square) => {
  return clickedSquares.value.includes(square);
};

const handleSquareClick = (square) => {
  clickedSquares.value.push(square);
};
</script>
