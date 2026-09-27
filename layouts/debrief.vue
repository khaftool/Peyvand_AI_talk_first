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
  shot?: string
  shotLabel?: string
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
  shot: '',
  shotLabel: '[ SCREENSHOT ]',
})
</script>

<template>
  <div class="slidev-layout debrief" :class="{ 'no-shot': !shot }">
    <TopStrip v-if="crumbs.length" :crumbs="crumbs" :active="active" :menu="true" :menu-to="menuTo" />
    <div class="head">
      <div class="swap">
        <p v-click-hide="1" class="ask">What did you see? →</p>
        <h1 v-click="1" class="result">{{ result }} <span class="o">{{ accent }}</span></h1>
      </div>
      <div v-if="$slots.beside" class="beside"><slot name="beside" /></div>
    </div>
    <div class="boxes">
      <div class="box">
        <span class="label">WHY</span>
        <p class="why">{{ why }}</p>
      </div>
      <div class="box accent">
        <span class="label o">FIX</span>
        <span class="tag orange fix-tag">{{ fixTag }}</span>
        <p class="fix mono">{{ fix }}</p>
        <p v-if="fixNote" class="fix-note">{{ fixNote }}</p>
      </div>
    </div>
    <div v-if="shot" class="shot">
      <ImageSlot :src="shot" :label="shotLabel" ratio="4/3" />
    </div>
    <slot />
  </div>
</template>

<style scoped>
.debrief {
  display: grid;
  grid-template-columns: 1fr 1fr 660px;
  grid-template-rows: auto 1fr;
  column-gap: 32px;
  row-gap: 40px;
  padding-top: 96px;
  padding-bottom: 64px;
}
.head { grid-column: 1 / 3; display: flex; gap: 40px; align-items: flex-start; min-height: 340px; }
.head .swap { flex: 1; }
.ask {
  font-family: var(--f-mono);
  font-size: 40px;
  color: var(--grey);
  margin: 12px 0 0;
}
.result { font-size: 84px; margin: 0; }
.beside { flex: none; }
.boxes { grid-column: 1 / 3; display: grid; grid-template-columns: 1fr 1fr; gap: 32px; align-self: start; }
.boxes .box { padding: 30px 34px 34px; min-height: 360px; }
.why { font-size: 31px; line-height: 1.4; margin: 0; }
.fix-tag { font-size: 20px; margin-bottom: 18px; white-space: normal; line-height: 1.25; }
.fix { font-size: 27px; line-height: 1.45; margin: 0; }
.fix-note { font-size: 26px; color: var(--grey); margin: 8px 0 0; }
.shot { grid-column: 3; grid-row: 1 / 3; padding-top: 12px; }

/* No screenshot: headline and boxes take the full width. */
.no-shot { grid-template-columns: 1fr 1fr; grid-template-rows: auto auto; align-content: center; row-gap: 64px; padding-top: 72px; }
.no-shot .head { min-height: 190px; }
.no-shot .result { font-size: 96px; max-width: 1500px; }
.no-shot .result .o { display: block; margin-top: 8px; }
.no-shot .boxes { gap: 48px; }
.no-shot .boxes .box { padding: 44px 52px 48px; min-height: 400px; }
.no-shot .boxes .box:first-child { border-width: 3px; }
.no-shot .boxes .box:first-child .label { color: #1F7A74; }
.no-shot .boxes .box.accent { border-width: 3px; }
.no-shot .label { font-size: 26px; margin-bottom: 22px; }
.no-shot .why { font-size: 44px; line-height: 1.3; font-weight: 600; }
.no-shot .fix-tag { font-size: 22px; margin-bottom: 24px; }
.no-shot .fix { font-size: 34px; }
.no-shot .fix-note { font-size: 30px; margin-top: 14px; }
</style>
