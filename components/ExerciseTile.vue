<script setup lang="ts">
defineProps<{
  code: string    // E3
  name: string    // Contradiction
  part: string    // P4
  to?: string     // route alias, defaults to the lowercase code
}>()
</script>

<template>
  <Link :to="to || code.toLowerCase()" class="tile-link">
    <span class="tile">
      <span class="code">{{ code }}</span>
      <span class="name">{{ name }}</span>
      <span class="part">{{ part }}</span>
    </span>
  </Link>
</template>

<style scoped>
.tile-link, :deep(a) { display: block; color: inherit; text-decoration: none; }
.tile {
  display: flex;
  align-items: center;
  gap: 22px;
  height: 80px;
  padding: 0 24px;
  border: 2px solid var(--ink);
  background: transparent;
  transition: border-color 120ms linear;
}
.tile:hover { border-color: var(--orange); }
.code {
  font-family: var(--f-mono);
  font-weight: 700;
  font-size: 28px;
  width: 58px;
}
.name { flex: 1; font-weight: 600; font-size: 30px; white-space: nowrap; }
.part {
  font-family: var(--f-mono);
  font-size: 20px;
  font-weight: 700;
  letter-spacing: 0.06em;
  color: var(--grey);
  white-space: nowrap;
}
</style>
