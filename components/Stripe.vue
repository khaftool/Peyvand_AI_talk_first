<script setup lang="ts">
withDefaults(defineProps<{ side?: 'left' | 'right', width?: number }>(), { side: 'left', width: 32 })
</script>

<template>
  <div class="stripe" :class="side" :style="{ width: `${width}px` }" aria-hidden="true" />
</template>

<style scoped>
.stripe { position: absolute; top: 0; bottom: 0; background: var(--orange); z-index: 2; }
.stripe.left { left: 0; }
.stripe.right { right: 0; }
</style>
