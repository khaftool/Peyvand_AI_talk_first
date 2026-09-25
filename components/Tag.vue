<script setup lang="ts">
withDefaults(defineProps<{
  tone?: 'ink' | 'orange'
  text: string
}>(), { tone: 'ink' })
</script>

<template>
  <span class="tag" :class="tone">{{ text }}</span>
</template>
