<script setup lang="ts">
withDefaults(defineProps<{
  tag?: string
  image?: string
  imageLabel?: string
}>(), { tag: '', image: '', imageLabel: '[ IMAGE ]' })
</script>

<template>
  <div class="slidev-layout divider">
    <div class="divider-grid">
      <div class="left">
        <span v-if="tag" class="tag ink">{{ tag }}</span>
        <slot />
      </div>
      <div class="right">
        <ImageSlot :src="image" :label="imageLabel" ratio="4/5" fit="cover" />
      </div>
    </div>
    <Checkerboard side="right" :cols="2" :cell="90" />
  </div>
</template>

<style scoped>
.divider { padding: 80px calc(180px + 96px) 80px 120px; }
.divider-grid {
  display: grid;
  grid-template-columns: 1fr 560px;
  gap: 80px;
  align-items: center;
  height: 100%;
}
.left :deep(h1) { font-size: 116px; margin-bottom: 36px; }
.left :deep(p) { margin: 0 0 18px; }
</style>
