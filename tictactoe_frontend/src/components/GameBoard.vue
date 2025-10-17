<script setup lang="ts">
import { computed } from 'vue'

type Player = 'X' | 'O'
type Cell = Player | null

const props = defineProps<{
  board: Cell[],
  current: Player,
  winningLine: number[] | null,
  disabled?: boolean
}>()

const emit = defineEmits<{
  (e: 'cell-click', index: number): void
}>()

const cells = computed(() => props.board)

function isWinning(index: number): boolean {
  return !!props.winningLine?.includes(index)
}

function onClick(index: number) {
  if (props.disabled) return
  emit('cell-click', index)
}
</script>

<template>
  <div class="board" role="grid" aria-label="TicTacToe board">
    <div
      v-for="(val, idx) in cells"
      :key="idx"
      class="cell"
      role="gridcell"
      :aria-label="`Cell ${idx+1}`"
      :data-player="val || ''"
      :data-turn="current"
      :class="{ disabled, win: isWinning(idx), filled: !!val }"
      @click="onClick(idx)"
    >
      <transition name="pop">
        <span v-if="val" class="mark" :class="val">{{ val }}</span>
      </transition>
    </div>
  </div>
</template>

<style scoped>
.board {
  width: min(92vw, 420px);
  aspect-ratio: 1/1;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  padding: 14px;
  background: linear-gradient(160deg, rgba(37,99,235,0.10), rgba(255,255,255,0.8));
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-md);
}

.cell {
  background: var(--color-surface);
  border: 1px solid rgba(17,24,39,0.06);
  border-radius: 16px;
  display: grid;
  place-items: center;
  cursor: pointer;
  position: relative;
  user-select: none;
  transition: transform 120ms ease, box-shadow 180ms ease, background 200ms ease, border-color 200ms ease, opacity 150ms ease;
  box-shadow: var(--shadow-sm);
  min-width: 0;
}

.cell:hover {
  transform: translateY(-1px);
  box-shadow: var(--shadow-md);
  background: linear-gradient(180deg, rgba(37,99,235,0.06), rgba(255,255,255,1));
}

.cell.filled {
  cursor: default;
  transform: none;
}

.cell.disabled {
  pointer-events: none;
  opacity: 0.7;
}

.cell.win {
  background: linear-gradient(180deg, rgba(245,158,11,0.18), rgba(255,255,255,1));
  border-color: rgba(245,158,11,0.45);
  box-shadow: 0 8px 18px rgba(245,158,11,0.28);
}

.mark {
  font-weight: 800;
  font-size: clamp(2.2rem, 10vw, 3.2rem);
  letter-spacing: 0.04em;
  line-height: 1;
}

.mark.X {
  color: var(--color-primary);
  text-shadow: 0 2px 6px rgba(37,99,235,0.25);
}

.mark.O {
  color: var(--color-secondary);
  text-shadow: 0 2px 6px rgba(245,158,11,0.25);
}

.pop-enter-active, .pop-leave-active {
  transition: transform 160ms ease, opacity 160ms ease;
}
.pop-enter-from, .pop-leave-to {
  transform: scale(0.7);
  opacity: 0;
}
</style>
