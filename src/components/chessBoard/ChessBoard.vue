<template>
  <div class="playpage">
    <!-- Board -->
    <main class="playpage__board">
      <div class="board-grid">
        <div v-for="rank in ranks" :key="rank" class="rank">
          <div
            v-for="file in files"
            :key="`${file}${rank}`"
            class="square"
            :class="{
              light: isLightSquare(file, rank),
              dark: !isLightSquare(file, rank),
              clicked: isClicked(`${file}${rank}`)
            }"
            @click="handleSquareClick(`${file}${rank}`)"
          >
            <span class="square-notation">{{ `${file}${rank}` }}</span>
          </div>
        </div>
      </div>
    </main>
    <!-- Aside -->
    <div class="playpage__notes">
      <h2 class="playpage__aside__title">Movimientos</h2>
      <div v-if="clickedSquares.length === 0" class="no-moves">No hay movimientos registrados</div>
      <ol v-else class="playpage__moves">
        <li v-for="(square, index) in clickedSquares" :key="index">
          {{ square }}
        </li>
      </ol>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const files = ["a", "b", "c", "d", "e", "f", "g", "h"];
const ranks = ["8", "7", "6", "5", "4", "3", "2", "1"];
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

<style scoped>
.playpage {
  display: flex;
  flex-direction: column;
  max-height: 100vh;
}

.playpage__board {
  padding: var(--gap-10);
}

.playpage__notes {
  overflow-y: scroll;
  top: 20px;
  flex: 1;
  padding: var(--gap-10);
  background:
		/* Shadow covers */ linear-gradient(white 30%, rgba(255, 255, 255, 0)), linear-gradient(rgba(255, 255, 255, 0), white 70%) 0 100%, /* Shadows */ linear-gradient(rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0)), linear-gradient(rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.4)) 0 100%;
  background-repeat: no-repeat;
  background-color: transparent;
  background-size: 100% 40px, 100% 40px, 100% 14px, 100% 14px;

  /* Opera doesn't support this in the shorthand */
  background-attachment: local, local, scroll, scroll;
}

.playpage__title {
}

.playpage__moves {
  display: flex;
  flex-direction: column-reverse;
}
.board {
  flex: 3;
}

.board-grid {
  display: grid;
  grid-template-columns: repeat(8, 1fr);
  aspect-ratio: 1/1;
  border: 1px solid #ccc;
}

.square {
  aspect-ratio: 1/1;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  position: relative;
  transition: opacity 0.2s;
}

.square:hover {
  opacity: 0.9;
}

.light {
  background-color: #fef3c7;
}

.dark {
  background-color: #92400e;
}

.clicked {
  box-shadow: inset 0 0 0 4px #3b82f6;
}

.square-notation {
  font-size: 0.75rem;
  opacity: 0.5;
}

.sidebar {
  flex: 1;
  background-color: #f3f4f6;
  padding: 1rem;
  border-radius: 0.5rem;
}

.sidebar h2 {
  font-size: 1.25rem;
  font-weight: bold;
  margin-bottom: 1rem;
}

.no-moves {
  color: #6b7280;
}

.moves-list {
  list-style: decimal;
  padding-left: 1.5rem;
}

.moves-list li {
  margin-bottom: 0.5rem;
}
</style>
