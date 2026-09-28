<script setup lang="ts">
// ── The Try-it sheet short link. Change it here, and only here. ──
const SHEET_LINK = 'https://tinyurl.com/peyvandai'

withDefaults(defineProps<{
  large?: boolean     // slide 15: big QR and big link
  caption?: string    // optional grey line under the link
}>(), { large: false, caption: '' })
</script>

<template>
  <div class="sheet-badge" :class="{ large }">
    <div class="qr">
      <ImageSlot src="/images/qr-sheet.png" label="QR" ratio="1/1" />
    </div>
    <div class="link mono">{{ SHEET_LINK }}</div>
    <div v-if="caption" class="cap"><em>{{ caption }}</em></div>
  </div>
</template>

<style scoped>
.sheet-badge { display: inline-flex; flex-direction: column; align-items: flex-start; }
.qr { width: 110px; }
.link { margin-top: 8px; font-size: 20px; font-weight: 700; letter-spacing: 0.02em; color: var(--ink); white-space: nowrap; }
.cap { margin-top: 4px; font-size: 22px; color: var(--grey); }
.large { align-items: center; }
.large .qr { width: 560px; }
.large .link { margin-top: 18px; font-size: 48px; }
.large .cap { font-size: 28px; }
</style>
