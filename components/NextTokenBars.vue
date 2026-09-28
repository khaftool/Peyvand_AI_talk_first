<!-- Slide 6: next-token odds. Shows `before`; when `lit`, animates to `after`. -->
<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(defineProps<{
  before: { word: string, pct: number }[]
  after: { word: string, pct: number }[]
  lit?: boolean
  highlight?: string   // row turned orange once lit
}>(), { lit: false, highlight: '' })

const ROW = 76
const MAX_W = 900
const words = computed(() => props.before.map(b => b.word))
const data = computed(() => (props.lit ? props.after : props.before))
const max = computed(() => Math.max(...props.before.map(b => b.pct), ...props.after.map(a => a.pct)))
function row(w: string) {
  const list = data.value
  const rank = list.findIndex(d => d.word === w)
  const pct = list[rank]?.pct ?? 0
  return { top: rank * ROW, width: (pct / max.value) * MAX_W, pct }
}
</script>

<template>
  <div class="bars" :style="{ height: `${words.length * ROW}px` }">
    <div
      v-for="w in words"
      :key="w"
      class="bar-row"
      :class="{ hi: lit && w === highlight }"
      :style="{ transform: `translateY(${row(w).top}px)` }"
    >
      <span class="word mono">{{ w }}</span>
      <span class="bar" :style="{ width: `${row(w).width}px` }" />
      <span class="pct mono">{{ row(w).pct }}%</span>
    </div>
  </div>
</template>

<style scoped>
.bars { position: relative; }
.bar-row {
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  gap: 18px;
  height: 64px;
  transition: transform 600ms ease-in-out;
}
.word { width: 170px; font-size: 30px; font-weight: 700; text-align: right; }
.bar { height: 44px; background: var(--ink); transition: width 600ms ease-in-out, background-color 300ms linear; }
.pct { font-size: 28px; color: var(--grey); }
.hi .bar { background: var(--orange); }
.hi .word { color: var(--orange); }
</style>
