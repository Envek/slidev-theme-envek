<script setup lang="ts">
const props = defineProps({
  footnoteClass: {
    type: String,
  },
})
</script>

<template>
  <div class="slidev-layout footnote-container w-full h-full grid gap-2 pb-0" style="grid-template-rows: 11fr 1fr;">
    <div class="place-self-stretch overflow-hidden">
      <slot />
    </div>
    <div class="self-end footnote" :class="props.footnoteClass">
      <slot name="footnote" />
    </div>
  </div>
</template>

<style>
.footnote-container > .footnote > p:first-child {
  margin-top: 0;
}
</style>
