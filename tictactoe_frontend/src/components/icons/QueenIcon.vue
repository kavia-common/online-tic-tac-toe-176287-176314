<template>
  <!-- Chess Queen icon, inherits color via currentColor -->
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
    <!-- Stylized queen silhouette (material-like) -->
    <path
      d="M18 19v-2.2c0-.53-.21-1.04-.59-1.41L14.83 12l2.29-2.29c.37-.37.58-.88.58-1.41V6.5c0-.83-.67-1.5-1.5-1.5-.59 0-1.11.34-1.35.84l-.71 1.48c-.12.25-.38.41-.66.41s-.54-.16-.66-.41l-.71-1.48A1.5 1.5 0 0 0 10.3 5c-.83 0-1.5.67-1.5 1.5v1.79c0 .53.21 1.04.59 1.41L11.68 12l-2.29 2.29c-.38.37-.59.88-.59 1.41V19H6c-.55 0-1 .45-1 1s.45 1 1 1h12c.55 0 1-.45 1-1s-.45-1-1-1h-0Zm-8 0v-1c0-.28.22-.5.5-.5h3c.28 0 .5.22.5.5v1H10Zm6-8.5-2 2-2-2-2 2-2-2V6.5c0-.28.22-.5.5-.5.19 0 .36.11.45.28l.71 1.48c.33.67 1.01 1.09 1.76 1.09s1.43-.42 1.76-1.09l.71-1.48A.5.5 0 0 1 16 6.5v4Z"
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
    title: 'Queen',
    ariaLabel: 'Queen icon',
  },
)
</script>
