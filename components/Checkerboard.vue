<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(defineProps<{
  side?: 'left' | 'right'
  cols?: number
  cell?: number
}>(), { side: 'right', cols: 3, cell: 90 })

const rows = computed(() => Math.ceil(1080 / props.cell))
const cells = computed(() => {
  const out: { x: number, y: number }[] = []
  for (let r = 0; r < rows.value; r++)
    for (let c = 0; c < props.cols; c++)
      out.push({ x: c * props.cell, y: r * props.cell })
  return out
})
</script>

<template>
  <svg
    class="checker"
    :class="side"
    :width="cols * cell"
    :height="rows * cell"
    :viewBox="`0 0 ${cols * cell} ${rows * cell}`"
    aria-hidden="true"
  >
    <rect x="0" y="0" :width="cols * cell" :height="rows * cell" fill="#F5EFE4" />
    <polygon
      v-for="(p, i) in cells"
      :key="i"
      :points="`${p.x},${p.y} ${p.x + cell},${p.y} ${p.x},${p.y + cell}`"
      fill="#141210"
    />
  </svg>
</template>

<style scoped>
.checker { position: absolute; top: 0; z-index: 2; display: block; }
.checker.right { right: 0; }
.checker.left { left: 0; }
</style>
