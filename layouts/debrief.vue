<script setup lang="ts">
withDefaults(defineProps<{
  crumbs?: string[]
  active?: number
  menuTo?: string
  result?: string   // ink part of the result headline
  accent?: string   // orange part of the result headline
  why?: string
  fixTag?: string   // e.g. PART 4 · CONSTRAINTS & RULES
  fix?: string      // the one-line fix, shown in mono
  fixNote?: string  // optional plain-text tail after the fix
}>(), {
  crumbs: () => [],
  active: -1,
  menuTo: 'menu',
  result: '',
  accent: '',
  why: '',
  fixTag: '',
  fix: '',
  fixNote: '',
})
</script>

<template>
  <div class="slidev-layout debrief">
    <TopStrip v-if="crumbs.length" :crumbs="crumbs" :active="active" :menu="true" :menu-to="menuTo" />
    <div class="head">
      <h1 class="result">{{ result }} <span v-if="accent" class="o">{{ accent }}</span></h1>
      <div v-if="$slots.beside" class="beside"><slot name="beside" /></div>
    </div>
    <div class="boxes">
      <div class="box why-box">
        <span class="label">WHY</span>
        <p class="why">{{ why }}</p>
      </div>
      <div class="box accent fix-box">
        <span class="label o">FIX</span>
        <span v-if="fixTag" class="tag orange fix-tag">{{ fixTag }}</span>
        <p class="fix mono">{{ fix }}</p>
        <p v-if="fixNote" class="fix-note">{{ fixNote }}</p>
      </div>
    </div>
    <slot />
  </div>
</template>

<style scoped>
/* Headline and boxes take the full width, centred vertically. */
.debrief {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto auto;
  align-content: center;
  column-gap: 32px;
  row-gap: 64px;
  padding-top: 72px;
  padding-bottom: 64px;
}
.head { grid-column: 1 / 3; display: flex; gap: 40px; align-items: flex-start; min-height: 150px; }
.result { flex: 1; font-size: 72px; margin: 0; max-width: 1600px; }
.result .o { display: block; margin-top: 8px; }
.beside { flex: none; }
.boxes { grid-column: 1 / 3; display: grid; grid-template-columns: 1fr 1fr; gap: 48px; align-self: start; }
.boxes .box { padding: 44px 52px 48px; min-height: 400px; border-width: 3px; }
.why-box .label { color: #1F7A74; }
.label { font-size: 26px; margin-bottom: 22px; }
.why { font-size: 44px; line-height: 1.3; font-weight: 600; margin: 0; }
.fix-tag { font-size: 22px; margin-bottom: 24px; white-space: normal; line-height: 1.25; }
.fix { font-size: 34px; line-height: 1.45; margin: 0; }
.fix-note { font-size: 30px; color: var(--grey); margin: 14px 0 0; }
</style>
