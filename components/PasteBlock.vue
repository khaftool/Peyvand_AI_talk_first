<script setup lang="ts">
defineProps<{ code?: string }>()
</script>

<template>
  <div class="paste">
    <div class="paste-text"><slot /></div>
    <span v-if="code" class="paste-code">{{ code }}</span>
  </div>
</template>

<style scoped>
.paste {
  display: flex;
  align-items: flex-start;
  gap: 24px;
  background: var(--paper);
  border-left: 6px solid var(--orange);
  padding: 9px 18px 9px 20px;
  margin: 6px 0 4px;
  font-family: var(--f-mono);
  font-size: 22px;
  line-height: 1.4;
  color: var(--ink);
}
.paste-text { flex: 1; white-space: pre-line; }
.paste-text :deep(p) { margin: 0; }
.paste-code {
  font-size: 18px;
  font-weight: 700;
  letter-spacing: 0.14em;
  color: var(--grey);
  padding-top: 4px;
}
</style>
