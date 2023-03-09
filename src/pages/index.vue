<script setup lang="ts">
useHead({
  title: 'tic-tac-toe',
})

const boardSize = 10
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

      if (g[i + 1][j - 1] === g[i][j] && g[i][j] === g[i - 1][j + 1])
        return g[i][j]
    }
  }

  return null
}

function handleCellClick(row: number, cell: number) {
  if (winner.value)
    return handleReset()

  if (grid.value[row][cell])
    return

  grid.value[row][cell] = current.value

  current.value = current.value === 'X' ? 'O' : 'X'

  winner.value = checkWinner()
}

function handleReset() {
  grid.value = initialGrid()
  current.value = 'X'
  winner.value = null
}
</script>

<template>
  <div
    class="h-screen flex flex-col items-center justify-center gap-6"
  >
    <div class="toolbar flex items-center justify-between gap-8">
      <h1 class="flex items-center gap-2">
        <div class="i-carbon-game-console text-5" />
        tic-tac-toe
      </h1>

      <div>next: {{ current }}</div>

      <button
        class="flex items-center gap-3 rounded-1 bg-gray-7 px-3 py-1 text-3 uppercase text-white hover:bg-gray-8"
        @click="handleReset"
      >
        <div class="i-carbon-reset" />
        reset
      </button>

      <dialog :open="!!winner" class="animate-jack-in-the-box cursor-pointer rounded-2 bg-gray-9 op-75 shadow-xl backdrop-blur-8 transition-opacity-666 hover:op-100" @click="handleReset">
        <div class="text-center text-8 font-bold">
          The winner is<br>
          <div class="text-14">
            {{ winner }}
          </div>

          <div
            class="flex items-center justify-center gap-3 rounded-1 bg-gray-7 px-3 py-1 text-3 uppercase text-white hover:bg-gray-8"
            @click="handleReset"
          >
            play again
          </div>
        </div>
      </dialog>
    </div>

    <div class="flex flex-col gap-2">
      <div v-for="row, r in grid" :key="r" class="flex gap-2">
        <div v-for="cell, c in row" :key="c">
          <div
            class="aspect-1 w-4rem cursor-pointer select-none bg-gray-7 text-center text-10 font-black hover:bg-gray-8"
            @click="handleCellClick(r, c)"
          >
            {{ cell }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
