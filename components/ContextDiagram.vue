<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(defineProps<{
  lit?: boolean   // draw the orange lines from YOUR PROMPT to every other band
  mini?: boolean  // tiny callback version: only the dashed (invisible) bands
}>(), { lit: false, mini: false })

type Kind = 'dashed' | 'solid' | 'prompt'
const ALL: { label: string, h: number, kind: Kind }[] = [
  { label: 'SYSTEM INSTRUCTIONS', h: 84, kind: 'dashed' },
  { label: 'MEMORY NOTES', h: 52, kind: 'dashed' },
  { label: 'PAST CONVERSATIONS', h: 60, kind: 'dashed' },
  { label: "TODAY'S DATE", h: 38, kind: 'dashed' },
  { label: 'CHAT HISTORY', h: 136, kind: 'solid' },
  { label: 'UPLOADED FILES', h: 96, kind: 'solid' },
  { label: 'SEARCH RESULTS', h: 84, kind: 'dashed' },
  { label: 'TOOL OUTPUTS', h: 56, kind: 'dashed' },
  { label: 'YOUR PROMPT', h: 34, kind: 'prompt' },
]

// geometry (px)
const PAD = 18
const HEAD = 46
const GAP = 8
const BAND_W = props.mini ? 220 : 560
const GUTTER = props.mini ? 0 : 120
const FRAME_W = PAD + BAND_W + GUTTER + PAD

const bands = computed(() => {
  const list = props.mini
    ? ALL.filter(b => b.kind === 'dashed').map(b => ({ ...b, h: Math.round(b.h * 0.42) }))
    : ALL
  let y = props.mini ? PAD : HEAD
  return list.map((b) => {
    const out = { ...b, y }
    y += b.h + GAP
    return out
  })
})

const frameH = computed(() => {
  const last = bands.value[bands.value.length - 1]
  return last.y + last.h + PAD
})

const prompt = computed(() => bands.value.find(b => b.kind === 'prompt'))
const paths = computed(() => {
  if (!prompt.value) return []
  const x = PAD + BAND_W
  const py = prompt.value.y + prompt.value.h / 2
  const others = bands.value.filter(b => b.kind !== 'prompt')
  return others.map((b, i) => {
    const by = b.y + b.h / 2
    const reach = 18 + (GUTTER - 30) * ((py - by) / (py - others[0].y - others[0].h / 2))
    return {
      d: `M ${x} ${py} C ${x + reach} ${py}, ${x + reach} ${by}, ${x} ${by}`,
      len: Math.round((py - by) + reach * 2),
      delay: i * 40,
    }
  })
})
</script>

<template>
  <div class="ctx" :class="{ mini, lit }">
    <div class="frame" :style="{ width: `${FRAME_W}px`, height: `${frameH}px` }">
      <span v-if="!mini" class="frame-label">CONTEXT</span>
      <div
        v-for="b in bands"
        :key="b.label"
        class="band"
        :class="b.kind"
        :style="{ top: `${b.y}px`, left: `${PAD}px`, width: `${BAND_W}px`, height: `${b.h}px` }"
      >
        <span v-if="!mini" class="band-label">{{ b.label }}</span>
      </div>
      <svg
        v-if="!mini"
        class="wires"
        :width="FRAME_W"
        :height="frameH"
        :viewBox="`0 0 ${FRAME_W} ${frameH}`"
        aria-hidden="true"
      >
        <path
          v-for="(p, i) in paths"
          :key="i"
          :d="p.d"
          :style="{ strokeDasharray: p.len, strokeDashoffset: lit ? 0 : p.len, transitionDelay: `${p.delay}ms` }"
        />
      </svg>
    </div>
    <template v-if="!mini">
      <div class="arrow">↓</div>
      <div class="model">MODEL</div>
    </template>
  </div>
</template>

<style scoped>
.ctx { display: inline-flex; flex-direction: column; align-items: flex-start; }
.frame {
  position: relative;
  border: 3px solid var(--ink);
}
.frame-label {
  position: absolute;
  top: 12px;
  left: 18px;
  font-family: var(--f-mono);
  font-weight: 700;
  font-size: 20px;
  letter-spacing: 0.18em;
}
.band {
  position: absolute;
  border: 2px solid var(--ink);
}
.band.dashed { border: 2px dashed var(--ink); }
.band.prompt { border: 3px solid var(--orange); }
.band-label {
  position: absolute;
  top: 50%;
  left: 14px;
  transform: translateY(-50%);
  font-family: var(--f-mono);
  font-weight: 700;
  font-size: 18px;
  line-height: 1;
  letter-spacing: 0.12em;
  white-space: nowrap;
}
.band.prompt .band-label { color: var(--orange); }
.wires { position: absolute; inset: -3px; overflow: visible; pointer-events: none; }
.wires path {
  fill: none;
  stroke: var(--orange);
  stroke-width: 2.5;
  transition: stroke-dashoffset 500ms linear;
}
.arrow {
  font-family: var(--f-mono);
  font-weight: 700;
  font-size: 44px;
  line-height: 1;
  margin: 8px 0 8px 18px;
  width: 560px;
  text-align: center;
}
.model {
  margin-left: 18px;
  width: 560px;
  background: var(--ink);
  color: var(--cream);
  font-family: var(--f-mono);
  font-weight: 700;
  font-size: 24px;
  letter-spacing: 0.18em;
  text-align: center;
  padding: 18px 0;
}
.mini .frame { border-width: 2px; }
.mini .band { border-width: 2px; }
</style>
