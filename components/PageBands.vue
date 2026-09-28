<!--
  The page bands: "what's on Amoo's page".
  solid  = you put it there
  dashed = the app added it, and you never see it
  key    = the row that decided the answer (orange); add `dashed: true` if the app added it
  empty  = an empty frame (e.g. NOTHING ELSE)
-->
<script setup lang="ts">
interface BandRow {
  label: string
  style?: 'solid' | 'dashed' | 'key' | 'empty'
  size?: 'thin' | 'normal' | 'tall'
  dashed?: boolean
}

const props = withDefaults(defineProps<{
  rows: BandRow[]
  title?: string
  compact?: boolean
  dense?: boolean    // extra-compact, for pages with many rows
}>(), { title: '', compact: false, dense: false })

const H = {
  full: { thin: 34, normal: 62, tall: 130 },
  compact: { thin: 46, normal: 62, tall: 116 },
  dense: { thin: 36, normal: 44, tall: 70 },
}
const h = (r: BandRow) => (props.dense ? H.dense : props.compact ? H.compact : H.full)[r.size || 'normal']
</script>

<template>
  <div class="page-bands" :class="{ compact: compact || dense, dense }">
    <span v-if="title" class="pb-title">{{ title }}</span>
    <div class="pb-rows">
      <div
        v-for="(r, i) in rows"
        :key="i"
        class="band"
        :class="[r.style || 'solid', { 'is-dashed': r.dashed }]"
        :style="{ minHeight: `${h(r)}px` }"
      >
        <span class="band-label">{{ r.label }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.page-bands { width: 100%; }
.pb-title {
  display: block;
  font-family: var(--f-mono);
  font-weight: 700;
  font-size: 20px;
  letter-spacing: 0.18em;
  margin-bottom: 12px;
}
.pb-rows { position: relative; display: flex; flex-direction: column; gap: 8px; }
.band {
  position: relative;
  display: flex;
  align-items: center;
  padding: 4px 14px;
  border: 2px solid var(--ink);
}
.band.dashed { border-style: dashed; }
.band.key { border: 3px solid var(--orange); }
.band.key.is-dashed { border-style: dashed; }
.band.empty { border: 2px solid var(--grey); }
.band-label {
  font-family: var(--f-mono);
  font-weight: 700;
  font-size: 18px;
  line-height: 1.2;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}
.band.key .band-label { color: var(--orange); }
.band.empty .band-label { color: var(--grey); }
.compact .band-label { font-size: 20px; letter-spacing: 0.06em; }
.compact .pb-rows { gap: 10px; }
.compact .band { padding: 6px 16px; }
.dense .pb-rows { gap: 7px; }
.dense .band { padding: 3px 14px; }
</style>
