<!-- Ink background, huge centered headline. In the slide:
     # FIRST LINE
     ## ORANGE SECOND LINE (optional) -->
<template>
  <div class="slidev-layout statement">
    <div class="statement-inner">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.statement {
  background: var(--ink);
  color: var(--cream);
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 80px 160px;
}
.statement-inner :deep(h1),
.statement-inner :deep(h2) {
  font-family: var(--f-head);
  font-weight: 900;
  text-transform: uppercase;
  font-size: 128px;
  line-height: 0.95;
  letter-spacing: -0.015em;
  margin: 0;
  color: var(--cream);
}
.statement-inner :deep(h2) { color: var(--orange); margin-top: 28px; }
</style>
