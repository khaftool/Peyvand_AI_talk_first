<script setup lang="ts">
const props = withDefaults(defineProps<{
  crumbs: string[]
  active?: number
  menu?: boolean
  menuTo?: string
}>(), {
  active: -1,
  menu: false,
  menuTo: 'menu',
})
</script>

<template>
  <div class="top-strip">
    <div class="crumbs">
      <template v-for="(c, i) in props.crumbs" :key="i">
        <span v-if="i > 0" class="sep">·</span>
        <span :class="{ on: i === (props.active < 0 ? props.crumbs.length - 1 : props.active) }">{{ c }}</span>
      </template>
    </div>
    <Link v-if="props.menu" :to="props.menuTo" class="menu">MENU ↩</Link>
  </div>
</template>

<style scoped>
.top-strip {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: var(--strip-h);
  background: var(--ink);
  color: var(--cream);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 var(--pad-x) 0 40px;
  font-family: var(--f-mono);
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  z-index: 5;
}
.crumbs { white-space: nowrap; }
.sep { margin: 0 0.7em; }
.on { color: var(--orange); }
.menu, :deep(a) {
  color: var(--cream);
  text-decoration: none;
  padding: 4px 10px;
  margin-right: -10px;
}
:deep(a:hover) { color: var(--orange); }
</style>
