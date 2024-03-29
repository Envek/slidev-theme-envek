<script setup>
import { ref, onUnmounted, computed, watch, nextTick } from 'vue'
import { useSlideContext } from '@slidev/client'
import martianImage from './assets/martian.svg';
import humanImage from './assets/human.svg';

const { $slidev } = useSlideContext();

// progress calculation
const intervalId = ref();
const startTime = ref(0);
const elapsedMinutes = ref(0)
const startAtSlide = computed(() => ($slidev.configs.progressBarStartSlide || 1));
const plannedDuration = computed(() => ($slidev.configs.talkDurationMinutes || 0));
const humanProgress = computed(() => ($slidev.nav.currentSlideNo - startAtSlide.value) / ($slidev.nav.total - startAtSlide.value));
const martianProgress = computed(() => Math.min(elapsedMinutes.value / plannedDuration.value, 1));
const progressBarEnabled = computed(() => $slidev.nav.currentSlideRoute.meta.slide.frontmatter.progressBar !== false && $slidev.nav.currentSlideNo >= startAtSlide.value);

// sizes handling
const containerRef = ref(null);
const martianRef = ref(null);
const humanRef = ref(null);
const sizes = ref({ containerWidth: 0, martianImageWidth: 0, humanImageWidth: 0 });
const humanTranslateX = computed(() =>
  (sizes.value.containerWidth - sizes.value.humanImageWidth - sizes.value.martianImageWidth) * humanProgress.value + sizes.value.martianImageWidth
);
const martianTranslateX = computed(() =>
(sizes.value.containerWidth - sizes.value.humanImageWidth - sizes.value.martianImageWidth) * martianProgress.value
);
const martianTranslateXInPrintCtx = computed(() => {
  return humanTranslateX.value - sizes.value.martianImageWidth
}
);
const progressBarScaleX = computed(() => humanTranslateX.value / sizes.value.containerWidth);
const humanScaleX = computed(() => $slidev.nav.clicksDirection >= 0 ? 100 : -100);

function updateElapsedMinutes() {
  elapsedMinutes.value = (Date.now() - startTime.value) / 60000
}

function updateSizes() {
  sizes.value = {
    containerWidth: containerRef.value?.clientWidth ?? 0,
    martianImageWidth: martianRef.value?.clientWidth ?? 0,
    humanImageWidth: humanRef.value?.clientWidth ?? 0,
  }
}

watch(() => $slidev.nav.currentSlideNo, (slideNo) => {
  if (startTime.value === 0 && slideNo >= startAtSlide.value) {
    startTime.value = Date.now();

    updateElapsedMinutes()
    intervalId.value = setInterval(updateElapsedMinutes, 1000)

    nextTick(updateSizes);
    window.addEventListener('resize', updateSizes)
  }
}, { immediate: true });

onUnmounted(() => {
  clearInterval(intervalId.value);
  window.removeEventListener('resize', updateSizes)
})

</script>

<template>
  <div v-show="progressBarEnabled">
    <div ref="containerRef" class="relative z-50 h-1 w-full bg-gray-400">
      <div class="absolute top-0 left-0 bottom-0 w-full bg-green-500 transform-origin-left transition-transform" :style="{ transform: `scaleX(${progressBarScaleX})` }" />
      <img ref="humanRef" :src="humanImage" class="w-8 absolute left-0 bottom-0 transition-transform" :style="{ transform: `translateX(${humanTranslateX}px) scaleX(${humanScaleX}%)` }" />
      <RenderWhen context="print">
        <img ref="martianRef" :src="martianImage" class="w-8 absolute left-0 bottom-0 transition-transform" :style="{ transform: `translateX(${martianTranslateXInPrintCtx}px)` }" />
        <template #fallback>
          <img ref="martianRef" :src="martianImage" class="w-8 absolute left-0 bottom-0 transition-transform" :style="{ transform: `translateX(${martianTranslateX}px)` }" />
        </template>
      </RenderWhen>
    </div> 
  </div>
</template>
