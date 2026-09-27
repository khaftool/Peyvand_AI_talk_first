<script setup lang="ts">
withDefaults(defineProps<{
  crumbs?: string[]
  active?: number
  menu?: boolean
  menuTo?: string
  tag?: string
  headline?: string
  story?: string
  predict?: string
}>(), {
  crumbs: () => [],
  active: -1,
  menu: false,
  menuTo: 'menu',
  tag: '▶ YOUR TURN',
  headline: '',
  story: '',
  predict: '',
})
</script>

<template>
  <div class="slidev-layout your-turn" :class="{ 'has-strip': crumbs.length }">
    <TopStrip v-if="crumbs.length" :crumbs="crumbs" :active="active" :menu="menu" :menu-to="menuTo" />
    <div class="main">
      <span class="tag orange">{{ tag }}</span>
      <h1 class="yt-headline">{{ headline }}</h1>
      <p v-if="story" class="story">{{ story }}</p>
      <p v-if="predict" class="predict">{{ predict }}</p>
      <div class="steps">
        <slot />
      </div>
    </div>
    <aside class="side">
      <img class="yt-icon" src="/images/exercise.svg" alt="Exercise">
      <div v-if="$slots.look" class="look">
        <span class="label">Look for:</span>
        <slot name="look" />
      </div>
      <slot name="extra" />
    </aside>
  </div>
</template>

<style scoped>
.your-turn {
  display: grid;
  grid-template-columns: 62fr 38fr;
  column-gap: 88px;
  padding-top: 84px;
  padding-bottom: 48px;
}
.your-turn.has-strip { padding-top: 92px; }
.yt-headline { font-size: 80px; margin-bottom: 14px; }
.story { color: var(--grey); font-size: 28px; line-height: 1.3; margin: 0 0 4px; }
.predict { color: var(--orange); font-style: italic; font-size: 28px; font-weight: 600; margin: 0 0 26px; }
.side { padding-top: 54px; display: flex; flex-direction: column; align-items: flex-start; gap: 48px; }
.yt-icon { width: 180px; height: 180px; display: block; }
.look { font-size: 28px; line-height: 1.45; }
.look :deep(p) { margin: 0 0 10px; }
.look :deep(ul) { margin: 0; padding: 0; list-style: none; }
.look :deep(li) { margin: 0 0 12px; padding-left: 30px; position: relative; }
.look :deep(li)::before { content: '→'; position: absolute; left: 0; color: var(--orange); font-family: var(--f-mono); }
.look :deep(strong) { font-weight: 700; }
</style>
