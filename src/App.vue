<script setup>
import { ref } from 'vue'

const currentPlayer = ref("X")

const board = ref(["", "", "", "", "", "", "", "", ""])
const winnerCondition = ["012", "345", "678", "036", "147", "258", "048", "246"]
const winner = ref("")

const nextPlayer = () => {
  currentPlayer.value === "X" ? currentPlayer.value = "O" : currentPlayer.value = "X";
}

const playerMove = (index) => {
  if (winner.value || board.value[index]) {
    return
  }
  board.value[index] = currentPlayer.value;
  winner.value = checkWinner();
  if (winner.value) {
    return
  }
  nextPlayer();
}

const newGame = () => {
  board.value = ["", "", "", "", "", "", "", "", ""];
  currentPlayer.value = "X";
  winner.value = "";
}

const checkWinner = () => {
  for (let k = 0; k < winnerCondition.length; k++) {
    const indexes = winnerCondition[k].split("");
    if (board.value[indexes[0]] !== "" && board.value[indexes[0]] === board.value[indexes[1]] && board.value[indexes[1]] === board.value[indexes[2]]) {
      return board.value[indexes[0]];
    }
  }
  return ""
}

</script>

<template>
  <div class="game-container">
    <h1>Крестики-нолики</h1>
    <div v-if="winner" class="status">Победил игрок <span>{{ winner }}</span></div>
    <div v-else class="status">Ход: <span>{{ currentPlayer }}</span></div>
    <div class="board-wrapper">
      <div class="board">
        <div @click="playerMove(index)" class="cell" v-for="( square , index) in board" :key="index">{{ square }}</div>
      </div>
    </div>
    <button @click="newGame">Новая игра</button>
  </div>
</template>

<style scoped>
body {
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  background-color: #f0f0f0;
}

.board-wrapper {
  display: flex;
  justify-content: center;
}

.game-container {
  text-align: center;
  background: white;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

h1 {
  color: #333;
  margin-bottom: 1rem;
}

.status {
  margin: 1rem 0;
  font-size: 1.2rem;
  font-weight: bold;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 5px;
  margin: 1rem auto;
}

.cell {
  width: 100px;
  height: 100px;
  background-color: #fff;
  border: 2px solid #333;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 3rem;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s;
}

.cell:hover {
  background-color: #f9f9f9;
}

.cell.x {
  color: #ff4757;
}

.cell.o {
  color: #2ed573;
}

#reset-btn {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  background-color: #333;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

#reset-btn:hover {
  background-color: #555;
}
</style>
