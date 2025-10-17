<script setup lang="ts">
import { computed } from 'vue'
import KnightIcon from './icons/KnightIcon.vue'
import QueenIcon from './icons/QueenIcon.vue'

type Player = 'X' | 'O'

const props = defineProps<{
  text: string
  winner: Player | null
  isDraw: boolean
  current: Player
  thinking?: boolean
  winningLine: number[] | null
}>()

const iconLabel = computed(() =>
  props.winner
    ? props.winner === 'X'
      ? 'X move (Knight)'
      : 'O move (Queen)'
    : props.current === 'X'
      ? 'X move (Knight)'
      : 'O move (Queen)',
)
</script>

<template>
  <div class="status">
    <div class="left">
      <div class="pill" :class="[{ win: !!winner, draw: isDraw }]">
        <span v-if="winner" class="dot win"></span>
        <span v-else-if="isDraw" class="dot draw"></span>
        <span v-else class="dot turn"></span>

        <!-- Icon representing current turn or winner -->
        <span class="who" aria-hidden="true" :class="winner ? (winner === 'X' ? 'X' : 'O') : (current === 'X' ? 'X' : 'O')">
          <KnightIcon
            v-if="(winner ?? current) === 'X'"
            :size="22"
            class="who-icon"
            :title="iconLabel"
            :aria-label="iconLabel"
          />
          <QueenIcon
            v-else
            :size="22"
            class="who-icon"
            :title="iconLabel"
            :aria-label="iconLabel"
          />
        </span>
        <strong :aria-label="text">{{ text }}</strong>
      </div>
    </div>
    <div class="right">
      <div v-if="thinking" class="thinking" aria-live="polite" aria-label="Computer thinking">
        <span class="bounce b1"></span>
        <span class="bounce b2"></span>
        <span class="bounce b3"></span>
        <span class="label">Computer thinking…</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.status {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  flex-wrap: wrap;
}
.left {
  display: flex;
  align-items: center;
  gap: 10px;
}
.pill {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 10px 14px;
  border-radius: 999px;
  background: rgba(37, 99, 235, 0.08);
  color: var(--color-text);
  border: 1px solid rgba(17,24,39,0.06);
  box-shadow: var(--shadow-sm);
}
.pill.win {
  background: rgba(245, 158, 11, 0.12);
  border-color: rgba(245,158,11,0.28);
}
.pill.draw {
  background: rgba(17, 24, 39, 0.06);
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  display: inline-block;
}
.dot.turn { background: var(--color-primary); }
.dot.win { background: var(--color-secondary); }
.dot.draw { background: var(--color-text); opacity: .4; }

.who {
  display: inline-flex;
  align-items: center;
  justify-content: center;
}
.who.X { color: var(--color-primary); }
.who.O { color: var(--color-secondary); }
.who-icon {
  display: inline-block;
  vertical-align: middle;
  filter: drop-shadow(0 1px 2px rgba(17,24,39,0.12));
}

.right {
  display: flex;
  align-items: center;
  gap: 10px;
}

.thinking {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 10px;
  border-radius: 10px;
  background: var(--color-bg);
  border: 1px solid rgba(17,24,39,0.06);
}
.bounce {
  width: 6px;
  height: 6px;
  background: var(--color-primary);
  border-radius: 50%;
  display: inline-block;
  animation: bounce 1.3s infinite ease-in-out both;
}
.b1 { animation-delay: 0s; }
.b2 { animation-delay: 0.15s; }
.b3 { animation-delay: 0.3s; }
.label {
  font-size: 12px;
  opacity: 0.8;
}

@keyframes bounce {
  0%, 80%, 100% { transform: scale(0); opacity: 0.4; }
  40% { transform: scale(1); opacity: 1; }
}
</style>
