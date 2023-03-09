<script setup lang="ts">
useHead({
  title: 'tic-tac-toe',
})

const boardSize = 8
const initialGrid = () => {
  const a: null[][] = []
  for (let i = 0; i < boardSize; i++) {
    a.push([])
    for (let j = 0; j < boardSize; j++)
      a[i].push(null)
  }
  return a

  // return [
  //   [null, null, null, null],
  //   [null, null, null, null],
  //   [null, null, null, null],
  //   [null, null, null, null],
  // ]
}

const grid = ref< (null | 'X' | 'O')[][]>(initialGrid())
const current = ref<'X' | 'O'>('X')

const winner = ref<null | 'X' | 'O'>(null)

function checkWinner(): 'X' | 'O' | null {
  const g = grid.value
  const size = g.length
  for (let i = 0; i < size; i++) {
    for (let j = 0; j < size; j++) {
      if (g[i][j] === null)
        continue

      if ((j - 1) >= 0 && (j + 1) < size && g[i][j - 1] === g[i][j] && g[i][j] === g[i][j + 1])
        return g[i][j]

      if ((i - 1) >= 0 && (i + 1) < size && g[i - 1][j] === g[i][j] && g[i][j] === g[i + 1][j])
        return g[i][j]

      if (!((i - 1) >= 0 && (i + 1) < size && (j - 1) >= 0 && (j + 1) < size))
        continue

      if (g[i - 1][j - 1] === g[i][j] && g[i][j] === g[i + 1][j + 1])
        return g[i][j]

      if (g[i + 1][j + 1] === g[i][j] && g[i][j] === g[i - 1][j - 1])
        return g[i][j]
    }
  }

  return null
}

function handleCellClick(row: number, cell: number) {
  if (winner.value)
    grid.value = initialGrid()

  grid.value[row][cell] = current.value

  current.value = current.value === 'X' ? 'O' : 'X'

  winner.value = checkWinner()
}

function handleReset() {
  grid.value = initialGrid()
  current.value = 'X'
}
</script>

<template>
  <div
    class="h-screen flex items-center justify-center"
  >
    <div>
      <div class="toolbar mb-4 flex justify-between">
        <h1>
          Game
        </h1>

        <div>Current player: {{ current }}</div>
        <div>Winner: {{ winner || '-' }}</div>
      </div>

      <button class="mb-4 rounded-1 bg-gray-7 px-3 py-1 text-white hover:bg-gray-8" @click="handleReset">
        reset
      </button>

      <div class="flex flex-col gap-2">
        <div v-for="row, r in grid" :key="r" class="flex gap-2">
          <div v-for="cell, c in row" :key="c">
            <div
              class="aspect-1 w-4rem cursor-pointer bg-gray-7 text-center text-10 font-black hover:bg-gray-8"
              @click="handleCellClick(r, c)"
            >
              {{ cell }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
