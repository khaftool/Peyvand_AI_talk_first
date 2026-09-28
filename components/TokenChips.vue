<script setup lang="ts">
withDefaults(defineProps<{
  tokens: { text: string, id: number | string }[]
  small?: boolean
  pick?: boolean      // every chip in orange (the chosen token)
  noIds?: boolean
}>(), { small: false, pick: false, noIds: false })

const TONES = ['t0', 't1', 't2', 't3']
</script>

<template>
  <div class="chips" :class="{ small }">
    <div v-for="(t, i) in tokens" :key="i" class="chip-wrap">
      <span class="chip" :class="pick ? 'pick' : TONES[i % 4]">{{ t.text }}</span>
      <span v-if="!noIds" class="chip-id">{{ t.id }}</span>
    </div>
  </div>
</template>

<style scoped>
.chips { display: flex; flex-wrap: wrap; gap: 10px 6px; align-items: flex-start; }
.chip-wrap { display: flex; flex-direction: column; align-items: flex-start; }
.chip {
  font-family: var(--f-mono);
  font-size: 40px;
  line-height: 1;
  padding: 12px 10px;
  border: 1.5px solid var(--ink);
  white-space: pre;
  color: var(--ink);
}
.t0 { background: #E6DCC9; }
.t1 { background: rgba(242, 107, 40, 0.25); }
.t2 { background: rgba(107, 102, 95, 0.2); }
.t3 { background: #FFFFFF; }
.pick { background: var(--orange); border-color: var(--orange); color: var(--ink); }
.chip-id { margin-top: 6px; font-family: var(--f-mono); font-size: 16px; color: var(--grey); letter-spacing: 0.04em; }
.small .chip { font-size: 18px; padding: 5px 4px; }
.small .chip-id { font-size: 14px; margin-top: 4px; }
.small { gap: 8px 4px; }
</style>
