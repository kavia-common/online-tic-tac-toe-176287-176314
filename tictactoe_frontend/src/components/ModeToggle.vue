<script setup lang="ts">
import { computed } from 'vue'

type Mode = 'pvp' | 'pvc'

const props = defineProps<{
  mode: Mode
}>()

const emit = defineEmits<{
  (e: 'update:mode', v: Mode): void
}>()

const isPvp = computed(() => props.mode === 'pvp')
const isPvc = computed(() => props.mode === 'pvc')

function setMode(m: Mode) {
  emit('update:mode', m)
}
</script>

<template>
  <div class="mode-toggle" role="group" aria-label="Game mode">
    <button
      class="toggle"
      :class="{ active: isPvp }"
      @click="setMode('pvp')"
      :aria-pressed="isPvp"
    >
      Vs Human
    </button>
    <button
      class="toggle"
      :class="{ active: isPvc }"
      @click="setMode('pvc')"
      :aria-pressed="isPvc"
    >
      Vs Computer
    </button>
  </div>
</template>

<style scoped>
.mode-toggle {
  display: inline-flex;
  background: var(--color-bg);
  border-radius: 12px;
  padding: 4px;
  gap: 6px;
  border: 1px solid rgba(17,24,39,0.08);
  box-shadow: var(--shadow-sm);
}
.toggle {
  background: transparent;
  color: var(--color-text);
  border: 0;
  border-radius: 10px;
  padding: 8px 12px;
  cursor: pointer;
  transition: background 160ms ease, color 160ms ease, transform 120ms ease;
}
.toggle:hover {
  background: rgba(37, 99, 235, 0.08);
}
.toggle.active {
  background: var(--color-primary);
  color: white;
  box-shadow: 0 6px 14px rgba(37, 99, 235, 0.25), 0 3px 7px rgba(37, 99, 235, 0.22);
}
</style>
