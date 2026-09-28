<!-- Slide 5: text → tokenizer → token IDs → model → odds → pick one, and the loop back. -->
<script setup lang="ts">
withDefaults(defineProps<{
  step?: number   // clicks so far: 0 shows stage 1 only; 1–5 add stages 2–6; 6 draws the loop
}>(), { step: 0 })

const BOX_W = 236
const GAP = 52
const BOX_H = 330
const x = (i: number) => i * (BOX_W + GAP)
const ids = [{ text: 'What', id: 3923 }, { text: ' computer', id: 6500 }, { text: ' should', id: 1288 }, { text: ' I', id: 358 }, { text: ' get', id: 636 }, { text: '?', id: 30 }]
const odds = [{ w: 'For', p: 46 }, { w: 'A', p: 31 }, { w: 'It', p: 12 }]
const loopY = BOX_H + 48
const loopPath = `M ${x(5) + BOX_W / 2} ${BOX_H} V ${loopY} H ${x(2) + BOX_W / 2} V ${BOX_H + 12}`
</script>

<template>
  <div class="pipe" :style="{ width: `${x(5) + BOX_W}px` }">
    <div class="stage" :class="{ hide: step < 0 }" :style="{ left: `${x(0)}px` }">
      <span class="label">TEXT</span>
      <p class="mono txt">"What computer should I get?"</p>
    </div>
    <div class="stage" :class="{ hide: step < 1 }" :style="{ left: `${x(1)}px` }">
      <span class="label">TOKENIZER</span>
      <p class="plain">splits it into pieces</p>
    </div>
    <div class="stage" :class="{ hide: step < 2 }" :style="{ left: `${x(2)}px` }">
      <span class="label">TOKEN IDS</span>
      <TokenChips :tokens="ids" small />
    </div>
    <div class="stage model" :class="{ hide: step < 3 }" :style="{ left: `${x(3)}px` }">
      <span class="label cream">MODEL</span>
      <p class="plain"><em>every token looks at every other token</em></p>
    </div>
    <div class="stage" :class="{ hide: step < 4 }" :style="{ left: `${x(4)}px` }">
      <span class="label">NEXT-TOKEN ODDS</span>
      <div v-for="o in odds" :key="o.w" class="odd">
        <span class="odd-w mono">{{ o.w }}</span>
        <span class="odd-bar" :style="{ width: `${o.p * 2.2}px` }" />
        <span class="odd-p mono">{{ o.p }}%</span>
      </div>
    </div>
    <div class="stage" :class="{ hide: step < 5 }" :style="{ left: `${x(5)}px` }">
      <span class="label o">PICK ONE</span>
      <TokenChips :tokens="[{ text: 'For', id: 1633 }]" pick />
    </div>

    <span v-for="i in 5" :key="`a${i}`" class="arrow" :class="{ hide: step < i }" :style="{ left: `${x(i) - GAP}px` }">→</span>

    <svg class="loop" :class="{ hide: step < 6 }" :width="x(5) + BOX_W" :height="loopY + 60" aria-hidden="true">
      <path :d="loopPath" />
      <polygon :points="`${x(2) + BOX_W / 2 - 9},${BOX_H + 22} ${x(2) + BOX_W / 2 + 9},${BOX_H + 22} ${x(2) + BOX_W / 2},${BOX_H + 6}`" />
    </svg>
    <p class="loop-label" :class="{ hide: step < 6 }" :style="{ left: `${x(2) + BOX_W / 2 + 20}px`, top: `${loopY + 8}px` }">
      <span class="o">↻</span> <em>added to the text, and the whole thing runs again.</em>
    </p>
  </div>
</template>

<style scoped>
.pipe { position: relative; height: 450px; }
.stage {
  position: absolute;
  top: 0;
  width: 236px;
  height: 330px;
  border: 2px solid var(--ink);
  padding: 20px 18px;
  transition: opacity 200ms linear;
}
.stage.model { background: var(--ink); color: var(--cream); }
.label.cream { color: var(--cream); }
.txt { font-size: 28px; line-height: 1.3; margin: 0; }
.plain { font-size: 28px; line-height: 1.3; margin: 0; }
.arrow {
  position: absolute;
  top: 120px;
  width: 52px;
  text-align: center;
  font-family: var(--f-mono);
  font-weight: 700;
  font-size: 36px;
  transition: opacity 200ms linear;
}
.odd { display: flex; align-items: center; gap: 8px; margin: 14px 0; }
.odd-w { width: 42px; font-size: 20px; font-weight: 700; }
.odd-bar { height: 18px; background: var(--ink); }
.odd-p { font-size: 18px; color: var(--grey); }
.loop { position: absolute; left: 0; top: 0; overflow: visible; pointer-events: none; transition: opacity 200ms linear; }
.loop path { fill: none; stroke: var(--orange); stroke-width: 3; }
.loop polygon { fill: var(--orange); }
.loop-label { position: absolute; margin: 0; font-size: 28px; white-space: nowrap; transition: opacity 200ms linear; }
.hide { opacity: 0; }
</style>
