<template>
  <!-- Chess Knight icon, inherits color via currentColor -->
  <svg
    xmlns="http://www.w3.org/2000/svg"
    :width="size"
    :height="size"
    viewBox="0 0 24 24"
    fill="currentColor"
    role="img"
    :aria-label="ariaLabel"
  >
    <title>{{ title }}</title>
    <!-- Stylized knight silhouette (material-like) -->
    <path
      d="M19 22H6c-1.1 0-2-.9-2-2 0-.74.4-1.39 1-1.73V16c0-1.65 1.35-3 3-3h2.59l-.88-1.76c-.18-.37-.27-.77-.27-1.17 0-1.03.52-2 .74-2.36l.07-.12c.12-.2.18-.43.18-.66 0-.69-.56-1.25-1.25-1.25-.38 0-.74.17-.98.47L7.6 6.72c-.38.51-1.09.62-1.6.24-.51-.38-.62-1.09-.24-1.6L7.7 2.65C8.44 1.69 9.66 1.13 10.93 1.25c1.97.18 3.57 1.78 3.75 3.75.04.45-.04.91-.24 1.31l-.06.12c-.08.16-.17.34-.26.53h1.88c1.65 0 3 1.35 3 3v3.5c.6.34 1 .99 1 1.73 0 1.1-.9 2-2 2h-1v2.27c.6.34 1 .99 1 1.73 0 1.1-.9 2-2 2ZM8 15c-.55 0-1 .45-1 1v2h9v-3H8Zm9-2V9.5c0-.28-.22-.5-.5-.5H12c-.38 0-.73.22-.9.56-.23.46-.1 1.03.23 1.7L12.26 13H17c0-.55.45-1 1-1ZM6 20h13c.55 0 1-.45 1-1s-.45-1-1-1H6c-.55 0-1 .45-1 1s.45 1 1 1Z"
    />
  </svg>
</template>

<script setup lang="ts">
withDefaults(
  defineProps<{
    size?: number | string
    title?: string
    ariaLabel?: string
  }>(),
  {
    size: 28,
    title: 'Knight',
    ariaLabel: 'Knight icon',
  },
)
</script>
