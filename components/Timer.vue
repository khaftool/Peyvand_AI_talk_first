<script setup lang="ts">
import { computed, onBeforeUnmount, ref } from 'vue'
import { useNav } from '@slidev/client'

const props = withDefaults(defineProps<{ minutes?: number }>(), { minutes: 3 })
const { isPrintMode } = useNav()

const total = computed(() => Math.round(props.minutes * 60))
const left = ref(total.value)
const running = ref(false)
let handle: ReturnType<typeof setInterval> | undefined

function stop() {
  if (handle) clearInterval(handle)
  handle = undefined
  running.value = false
}
function reset() {
  stop()
  left.value = total.value
}
function start() {
  const endAt = Date.now() + left.value * 1000
  running.value = true
  handle = setInterval(() => {
    left.value = Math.max(0, Math.ceil((endAt - Date.now()) / 1000))
    if (left.value === 0) stop()
  }, 200)
}
function toggle() {
  if (isPrintMode.value) return
  // idle → start · running → reset · finished → reset
  if (!running.value && left.value === total.value) start()
  else reset()
}
onBeforeUnmount(stop)

const done = computed(() => !isPrintMode.value && left.value === 0)
const warn = computed(() => !isPrintMode.value && left.value > 0 && left.value <= 30 && running.value)
const display = computed(() => {
  if (isPrintMode.value) {
    const m = Math.floor(total.value / 60), s = total.value % 60
    return `${String(m).padStart(2, '0')}:${String(s).padStart(2, '0')}`
  }
  if (done.value) return 'TIME'
  const m = Math.floor(left.value / 60), s = left.value % 60
  return `${String(m).padStart(2, '0')}:${String(s).padStart(2, '0')}`
})
</script>

<template>
  <button class="timer" :class="{ warn, done, running }" type="button" @click.stop="toggle" :title="running ? 'Click to reset' : 'Click to start'">
    <span class="face">{{ display }}</span>
  </button>
</template>

<style scoped>
.timer {
  position: relative;
  display: inline-block;
  padding: 0;
  border: 0;
  background: none;
  cursor: pointer;
  font: inherit;
  margin: 0 8px 8px 0;
}
.timer::before {
  content: '';
  position: absolute;
  inset: 0;
  transform: translate(8px, 8px);
  background: var(--orange);
}
.face {
  position: relative;
  display: block;
  min-width: 300px;
  padding: 14px 30px 12px;
  border: 3px solid var(--ink);
  background: var(--cream);
  font-family: var(--f-mono);
  font-weight: 700;
  font-size: 92px;
  line-height: 1;
  letter-spacing: 0.02em;
  text-align: center;
  color: var(--ink);
  font-variant-numeric: tabular-nums;
}
.warn .face, .done .face {
  background: var(--orange);
  border-color: var(--ink);
}
.warn::before, .done::before { background: var(--ink); }
</style>
