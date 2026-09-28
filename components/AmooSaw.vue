<script setup lang="ts">
interface BandRow {
  label: string
  style?: 'solid' | 'dashed' | 'key' | 'empty'
  size?: 'thin' | 'normal' | 'tall'
  dashed?: boolean
}

defineProps<{
  rows: BandRow[]
  why?: string
  dense?: boolean
}>()
</script>

<template>
  <div class="box amoo-saw">
    <span class="label">AMOO ONLY SAW</span>
    <PageBands :rows="rows" compact :dense="dense" />
    <p v-if="why" class="saw-why">{{ why }}</p>
  </div>
</template>

<style scoped>
.amoo-saw { padding: 28px 32px 32px; border-width: 3px; }
.saw-why {
  margin: 24px 0 0;
  color: var(--grey);
  font-size: 28px;
  line-height: 1.35;
}
</style>
