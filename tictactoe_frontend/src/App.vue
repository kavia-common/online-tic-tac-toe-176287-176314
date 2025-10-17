<script setup lang="ts">
import { ref, computed, watch, nextTick } from 'vue'
import ModeToggle from './components/ModeToggle.vue'
import GameBoard from './components/GameBoard.vue'
import StatusBar from './components/StatusBar.vue'
import './styles/theme.css'

type Player = 'X' | 'O'
type Cell = Player | null
type Mode = 'pvp' | 'pvc'

const mode = ref<Mode>('pvc')
const board = ref<Cell[]>(Array(9).fill(null))
const current = ref<Player>('X')
const winner = ref<Player | null>(null)
const winningLine = ref<number[] | null>(null)
const isDraw = ref(false)
const computerThinking = ref(false)

const canPlay = computed(() => !winner.value && !isDraw.value)
const statusText = computed(() => {
  if (winner.value) return `Winner: ${winner.value}`
  if (isDraw.value) return 'Draw game'
  return `Turn: ${current.value}`
})

function lines(): number[][] {
  return [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ]
}

function checkWinner(b: Cell[]): { winner: Player | null; line: number[] | null } {
  for (const [a, c, d] of lines()) {
    if (b[a] && b[a] === b[c] && b[a] === b[d]) {
      return { winner: b[a], line: [a, c, d] }
    }
  }
  return { winner: null, line: null }
}

function checkDraw(b: Cell[]): boolean {
  return b.every((c) => c !== null)
}

function emptyIndices(b: Cell[]): number[] {
  const idx: number[] = []
  b.forEach((v, i) => v === null && idx.push(i))
  return idx
}

// Simple AI: try win, else block, else center, corner, side
function bestMove(ai: Player, b: Cell[]): number | null {
  const human: Player = ai === 'X' ? 'O' : 'X'
  // 1. Winning move
  for (const i of emptyIndices(b)) {
    const copy = b.slice()
    copy[i] = ai
    if (checkWinner(copy).winner === ai) return i
  }
  // 2. Block human winning move
  for (const i of emptyIndices(b)) {
    const copy = b.slice()
    copy[i] = human
    if (checkWinner(copy).winner === human) return i
  }
  // 3. Center
  if (b[4] === null) return 4
  // 4. Corners
  const corners = [0, 2, 6, 8]
  const freeCorners = corners.filter((i) => b[i] === null)
  if (freeCorners.length) return freeCorners[Math.floor(Math.random() * freeCorners.length)]
  // 5. Sides
  const sides = [1, 3, 5, 7]
  const freeSides = sides.filter((i) => b[i] === null)
  if (freeSides.length) return freeSides[Math.floor(Math.random() * freeSides.length)]
  return null
}

// PUBLIC_INTERFACE
function resetGame() {
  /** Reset game to initial state. */
  board.value = Array(9).fill(null)
  current.value = 'X'
  winner.value = null
  winningLine.value = null
  isDraw.value = false
  computerThinking.value = false
}

// PUBLIC_INTERFACE
function setMode(newMode: Mode) {
  /** Set game mode and start a fresh game. */
  mode.value = newMode
  resetGame()
}

function applyMove(index: number, player: Player) {
  if (board.value[index] !== null) return
  board.value[index] = player
  const { winner: w, line } = checkWinner(board.value)
  if (w) {
    winner.value = w
    winningLine.value = line
    return
  }
  if (checkDraw(board.value)) {
    isDraw.value = true
    return
  }
  current.value = player === 'X' ? 'O' : 'X'
}

async function aiTurnIfNeeded() {
  if (!canPlay.value) return
  if (mode.value !== 'pvc') return
  if (current.value !== 'O') return // human is X, computer is O
  computerThinking.value = true
  await new Promise((r) => setTimeout(r, 350))
  const idx = bestMove('O', board.value) ?? emptyIndices(board.value)[0]
  if (idx !== null && idx !== undefined) {
    applyMove(idx, 'O')
  }
  computerThinking.value = false
}

function onCellClick(index: number) {
  if (!canPlay.value) return
  if (board.value[index] !== null) return
  if (mode.value === 'pvc' && current.value === 'O') return // prevent user during AI turn
  applyMove(index, current.value)
  nextTick().then(aiTurnIfNeeded)
}

watch(mode, () => {
  // auto-AI start if AI goes first in some future variation
  nextTick().then(aiTurnIfNeeded)
})
</script>

<template>
  <div class="container">
    <div class="card" style="padding: 24px">
      <div style="display:flex; align-items:center; justify-content: space-between; gap: 12px; flex-wrap: wrap;">
        <div style="display:flex; align-items:center; gap:12px;">
          <div class="badge">
            <span style="width:8px;height:8px;border-radius:50%;background:var(--color-primary);display:inline-block"></span>
            TicTacToe
          </div>
          <span style="opacity:.7">Ocean Professional</span>
        </div>
        <ModeToggle :mode="mode" @update:mode="setMode" />
      </div>

      <div style="height: 20px"></div>

      <div class="grid-outer">
        <GameBoard
          :board="board"
          :current="current"
          :winning-line="winningLine"
          :disabled="!canPlay || (mode==='pvc' && computerThinking)"
          @cell-click="onCellClick"
        />
      </div>

      <div style="height: 16px"></div>

      <StatusBar
        :text="statusText"
        :winner="winner"
        :is-draw="isDraw"
        :current="current"
        :thinking="computerThinking"
        :winning-line="winningLine"
      />

      <div style="display:flex; justify-content:center; margin-top:16px;">
        <button class="btn ghost" @click="resetGame" aria-label="Start a new game">
          New Game
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>
