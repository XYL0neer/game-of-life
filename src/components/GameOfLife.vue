<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue';

const props = defineProps<{ height: number, width: number, initialLifePool: number }>()
const interval = ref<number | undefined>(undefined)

const width = props.width
const height = props.height
const boardSize = width * height

const elements = Array(boardSize).fill(0).map((_, idx) => {
  const el = document.createElement("div")
  el.classList.add("cell")
  el.id = idx.toString()
  el.innerText = ""
  return el
})
const aliveCells = new Array(boardSize).fill(false)


onUnmounted(() => {
  clearInterval(interval.value)
  interval.value = undefined
})

onMounted(() => {
  const gameBoard = document.getElementById("game-board")
  if (!gameBoard) return
  gameBoard.style.gridTemplateRows = `repeat(${height}, 1rem)`
  gameBoard.style.gridTemplateColumns = `repeat(${width}, 1rem)`
  gameBoard.append(...elements)

  const maxPosition = boardSize - 1
  for (let i = 0; i < props.initialLifePool; i++) {
    const alivePos = generateRandom(maxPosition)
    aliveCells[alivePos] = true
    elements[alivePos]?.classList.add("alive")
  }

  interval.value = setInterval(nextTick, 500)
})

function nextTick() {
  console.time("tick")
  // Any live cell with fewer than two live neighbours dies, as if by underpopulation.
  // Any live cell with two or three live neighbours lives on to the next generation.
  // Any live cell with more than three live neighbours dies, as if by overpopulation.
  // Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.
  let hadActivity = false
  for (let i = 0; i < boardSize; i++) {
    let livingNeighborsCount = countLivingNeighbors(i)

    if (aliveCells[i] && (livingNeighborsCount < 2 || livingNeighborsCount > 3)) {
      aliveCells[i] = false
      elements[i]?.classList.remove("alive")
      hadActivity = true
    } else if (!aliveCells[i] && livingNeighborsCount === 3) {
      aliveCells[i] = true
      elements[i]?.classList.add("alive")
      hadActivity = true
    }
  }

  console.timeEnd("tick")

  if (!hadActivity) {
    clearInterval(interval.value)
    interval.value = undefined
    console.log(">>>>> Final state reached")
  }
}

function countLivingNeighbors(pos: number) {
  let count = 0
  if (pos % width < width - 1) {
    if (aliveCells[pos + 1]) count++
    if (aliveCells[pos + 1 + width] && pos + 1 + width < boardSize) count++
    if (aliveCells[pos + 1 - width] && pos + 1 - width >= 0) count++
  }

  if (pos - 1 >= 0) {
    if (aliveCells[pos - 1]) count++
    if (aliveCells[pos - 1 + width] && pos - 1 + width < boardSize) count++
    if (aliveCells[pos - 1 - width] && pos - 1 - width >= 0) count++
  }
  if (aliveCells[pos + width] && pos + width < boardSize) count++
  if (aliveCells[pos - width] && pos - width >= 0) count++

  return count
}

function generateRandom(max: number) {
  const rand = Math.random() * max
  return Math.floor(rand)
}

function togglePlay() {
  if (interval.value === undefined) {
    interval.value = setInterval(nextTick, 500)
  } else {
    clearInterval(interval.value)
    interval.value = undefined
  }
}
</script>

<template>
  <button @click="togglePlay">Play</button>
  <div id="game-board"></div>
</template>

<style>
#game-board {
  display: grid;
  gap: .1rem;
}

.cell {
  background-color: whitesmoke;
}

.cell.alive {
  background-color: yellowgreen;
}
</style>
