<script setup lang="ts">
import SquareBoard from "./SquareBoard.vue";
import { computed, reactive, ref } from "vue";

interface BoardItem {
  player: string | null;
  index: number;
}
const winnerLines = [
  // строки
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  // столбцы
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  // диагонали
  [0, 4, 8],
  [2, 4, 6],
];
const board = reactive<BoardItem[]>([
  { player: null, index: 0 },
  { player: null, index: 1 },
  { player: null, index: 2 },
  { player: null, index: 3 },
  { player: null, index: 4 },
  { player: null, index: 5 },
  { player: null, index: 6 },
  { player: null, index: 7 },
  { player: null, index: 8 },
] as const);
const player = ref<string>("X");
const isPlayerComputer = ref<boolean>(false);

const playerXScore = ref<number[]>([]);
const playerYScore = ref<number[]>([]);

const checkWinners = (playerArray: number[]) => {
  return winnerLines.some((elem) =>
    elem.every((item) => playerArray.includes(item)),
  );
};

const getComputerStep = () => {
  const clearBoard = board.filter((elem) => !elem.player);
  const randomClearCell =
    clearBoard[Math.floor(Math.random() * clearBoard.length)];
  return randomClearCell;
};

const handleCellClickOnItem = (elem: { player: string; index: number }) => {
  const cell = board[elem.index];
  if (!cell || cell.player !== null) return;

  // Ход игрока
  cell.player = player.value;
  if (player.value === "X") {
    playerXScore.value.push(elem.index);
  } else {
    playerYScore.value.push(elem.index);
  }
  player.value = player.value === "X" ? "O" : "X";

  // Ход компьютера
  if (isPlayerComputer.value && !winner.value && !isDraw.value) {
    setTimeout(() => {
      const randomClearCell = getComputerStep();
      if (randomClearCell) {
        const computerCell = board[randomClearCell.index];
        if (computerCell) {
          computerCell.player = player.value;
          if (player.value === "X") {
            playerXScore.value.push(computerCell.index);
          } else {
            playerYScore.value.push(computerCell.index);
          }
        }
        player.value = player.value === "X" ? "O" : "X";
      }
    }, 100);
  }
};

const resetGame = () => {
  player.value = "X";
  playerXScore.value = [];
  playerYScore.value = [];
  board.forEach((cell) => {
    cell.player = null;
  });
};

const winner = computed(() => {
  if (checkWinners(playerXScore.value)) return "X";
  if (checkWinners(playerYScore.value)) return "O";
  return null;
});
const isDraw = computed(() => {
  return board.every((cell) => cell.player !== null) && !winner.value;
});
</script>

<template>
  <div class="container">
    <SquareBoard
      v-for="elem in board"
      :key="elem.index"
      :elem="elem"
      :player="player"
      @clickeditem="handleCellClickOnItem"
    />
  </div>
  <div class="controlBox">
    <h4 v-if="winner" style="color: green">Победил: {{ winner }}</h4>
    <h4 v-else-if="isDraw" style="color: orange">Ничья!</h4>
    <h4 v-else>Текущий игрок: {{ player }}</h4>
    <div class="smallContainer">
      <p>Играть с компьютером</p>
      <input type="checkbox" v-model="isPlayerComputer" />
    </div>
    <button @click="resetGame">Сбросить игру</button>
  </div>
</template>

<style lang="css" scoped>
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  max-width: 1200px;
  margin: 0 auto;
}

.controlBox {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.smallContainer {
  display: flex;
}
</style>
