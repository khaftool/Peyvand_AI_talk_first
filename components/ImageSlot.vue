<script setup lang="ts">
import { computed, ref, watch } from 'vue'

const props = withDefaults(defineProps<{
  src?: string
  label?: string
  ratio?: string        // e.g. "16/9", "3/4", "1/1"
  fit?: 'contain' | 'cover'
  caption?: boolean     // also print the label under the slot
  screenshot?: boolean  // show the image as-is: no blending, framed like a screenshot
}>(), {
  label: '[ IMAGE ]',
  ratio: '16/9',
  fit: 'contain',
  caption: false,
  screenshot: false,
})

const loaded = ref(false)
const failed = ref(false)
watch(() => props.src, () => { loaded.value = false; failed.value = false })

const url = computed(() => {
  if (!props.src) return ''
  if (/^(https?:|data:)/.test(props.src)) return props.src
  const base = (import.meta.env.BASE_URL || '/').replace(/\/$/, '')
  return base + (props.src.startsWith('/') ? props.src : `/${props.src}`)
})
</script>

<template>
  <figure class="image-slot" :class="{ screenshot }">
    <div class="frame" :class="{ empty: !loaded }" :style="{ aspectRatio: ratio }">
      <img
        v-if="url && !failed"
        v-show="loaded"
        :src="url"
        :alt="label"
        :style="{ objectFit: fit, mixBlendMode: screenshot ? 'normal' : 'darken' }"
        @load="loaded = true"
        @error="failed = true"
      >
      <span v-if="!loaded && !caption" class="slot-label">{{ label }}</span>
    </div>
    <figcaption v-if="caption" class="slot-caption">{{ label }}</figcaption>
  </figure>
</template>

<style scoped>
.image-slot { margin: 0; width: 100%; }
.frame {
  position: relative;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.frame.empty { border: 3px dotted var(--dot); }
img { width: 100%; height: 100%; display: block; }
.screenshot .frame { overflow: visible; }
.screenshot img {
  width: auto;
  height: auto;
  max-width: calc(100% - 12px);
  max-height: calc(100% - 12px);
  border: 2px solid var(--ink);
  box-shadow: 10px 10px 0 var(--ink);
}
.slot-label {
  font-family: var(--f-mono);
  font-size: 20px;
  letter-spacing: 0.08em;
  color: var(--grey);
  text-align: center;
  padding: 0 16px;
}
.slot-caption {
  margin-top: 10px;
  font-family: var(--f-mono);
  font-size: 18px;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--ink);
}
</style>
