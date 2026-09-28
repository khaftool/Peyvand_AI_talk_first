<!-- Slide 7: the context window as one horizontal bar of page bands, with room left over. -->
<script setup lang="ts">
const segs = [
  { label: 'SYSTEM INSTRUCTIONS', w: 210, style: 'dashed' },
  { label: 'MEMORY', w: 130, style: 'dashed' },
  { label: 'CHAT HISTORY', w: 400, style: 'solid' },
  { label: 'FILES', w: 230, style: 'solid' },
  { label: 'YOUR PROMPT', w: 96, style: 'key' },
  { label: 'ANSWER', w: 220, style: 'answer' },
]
</script>

<template>
  <div class="cw">
    <span class="label">CONTEXT WINDOW · MAX SIZE</span>
    <div class="cw-frame">
      <div v-for="s in segs" :key="s.label" class="seg" :class="s.style" :style="{ width: `${s.w}px` }">
        <span class="seg-label">{{ s.label }}</span>
        <span v-if="s.style === 'answer'" class="seg-note"><em>space for the reply</em></span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.cw { width: 100%; }
.cw-frame {
  display: flex;
  gap: 8px;
  height: 128px;
  padding: 10px;
  border: 3px solid var(--ink);
}
.seg { position: relative; flex: none; border: 2px solid var(--ink); padding: 10px 10px; }
.seg.dashed { border-style: dashed; }
.seg.key { border: 3px solid var(--orange); }
.seg.answer {
  border: 2px solid var(--grey);
  background-image: repeating-linear-gradient(135deg, rgba(107, 102, 95, 0.35) 0 2px, transparent 2px 12px);
}
.seg-label {
  display: block;
  font-family: var(--f-mono);
  font-weight: 700;
  font-size: 18px;
  line-height: 1.15;
  letter-spacing: 0.08em;
}
.seg.key .seg-label { color: var(--orange); }
.seg.answer .seg-label { color: var(--grey); }
.seg-note { display: block; margin-top: 8px; font-size: 20px; color: var(--grey); background: var(--cream); padding: 0 4px; width: fit-content; }
</style>
