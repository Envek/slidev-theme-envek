<script setup lang="ts">
const props = defineProps({
  url: {
    type: String,
  },
  caption: {
    type: String,
  },
  class: {
    type: String,
  },
})
</script>

<template>
  <figure class="qr-code-container px-2" :class="props.class">
    <a :href="props.url">
      <qr-code-vue :value="props.url" class="object-contain max-w-full max-h-full w-full h-full" render-as="svg" margin="1" />
      <figcaption class="text-center py-1">{{props.caption}}</figcaption>
    </a>
  </figure>
</template>

<style scoped>
  .qr-code-container figcaption {
    font-size: 0.5rem;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    font-stretch: 3%;
  }
</style>
