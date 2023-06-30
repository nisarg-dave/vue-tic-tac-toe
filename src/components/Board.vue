<script setup>
import { ref, computed } from 'vue'
import Square from './Square.vue'

const squares = ref(Array(9).fill(null))
const currentPlayer = ref(Math.round(Math.random()) === 1 ? 'X' : 'O')
const lines = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6]
]

const changePlayer = () => {
  if (currentPlayer.value === 'X') {
    currentPlayer.value = 'O'
  } else {
    currentPlayer.value = 'X'
  }
}

const setSquareValue = (index) => {
  const newData = squares.value.map((val, i) => {
    if (i === index) {
      return currentPlayer.value
    }
    return val
  })
  squares.value = newData
  changePlayer()
}

const reset = () => {
  squares.value = Array(9).fill(null)
  currentPlayer.value = Math.round(Math.random() * 1) === 1 ? 'X' : 'O'
}

const winner = computed(() => {
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]
    if (
      squares.value[a] &&
      squares.value[a] === squares.value[b] &&
      squares.value[a] === squares.value[c]
    ) {
      return squares.value[a]
    }
    if (!winner.value && !squares.value.filter((square) => !square).length) {
      return 'DRAW'
    }
  }
  return null
})
</script>

<template>
  <div>
    <p v-show="!winner && winner !== 'DRAW'">Hey {{ currentPlayer }}, it's your turn</p>
    <p v-show="winner && winner !== 'DRAW'">Congratulations {{ winner }}!</p>
    <p v-show="winner === 'DRAW'">We have a draw!</p>
    <div class="grid">
      <div v-for="(square, index) in squares">
        <Square
          :winner="winner"
          :key="index"
          :index="index"
          :value="square"
          @set-square-value="setSquareValue"
        />
      </div>
    </div>
    <div>
      <button class="reset" @click="reset">Reset</button>
    </div>
  </div>
</template>

<style scoped>
.grid {
  display: grid;
  gap: 1px;
  grid-template-columns: repeat(3, 1fr);
}
.reset {
  margin-top: 0.5rem;
  font-size: 1rem;
  background-color: aqua;
  border: none;
  border-radius: 6px;
  padding: 0.5rem;
  cursor: pointer;
}
</style>
