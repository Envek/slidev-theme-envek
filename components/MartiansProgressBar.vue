<script setup>
import { ref, onUnmounted, computed, watch, nextTick } from 'vue'
import { useSlideContext } from '@slidev/client'

const { $slidev } = useSlideContext();

// progress calculation
const intervalId = ref();
const startTime = ref(0);
const elapsedMinutes = ref(0)
const startAtSlide = computed(() => ($slidev.configs.martiansProgressBarStartSlide || 1));
const plannedDuration = computed(() => ($slidev.configs.martiansProgressBarDuration || 0));
const humanProgress = computed(() => ($slidev.nav.currentSlideNo - startAtSlide.value) / ($slidev.nav.total - startAtSlide.value));
const martianProgress = computed(() => Math.min(elapsedMinutes.value / plannedDuration.value, 1));

// sizes handling
const containerRef = ref(null);
const martianRef = ref(null);
const humanRef = ref(null);
const sizes = ref({ containerWidth: 0, martianImageWidth: 0, humanImageWidth: 0 });
const martianTranslateX = computed(() =>
  (sizes.value.containerWidth - sizes.value.martianImageWidth) * martianProgress.value
);
const humanTranslateX = computed(() =>
  (sizes.value.containerWidth - sizes.value.humanImageWidth - sizes.value.martianImageWidth) * humanProgress.value + sizes.value.martianImageWidth
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
  <div v-show="$slidev.nav.currentSlideNo >= startAtSlide">
    <div ref="containerRef" class="relative z-50 h-2 w-full bg-gray-400">
      <div class="absolute top-0 left-0 bottom-0 w-full bg-green-500 transform-origin-left transition-transform" :style="{ transform: `scaleX(${progressBarScaleX})` }" />
      <img ref="martianRef" src="/graphics/human.svg" class="w-8 absolute left-0 bottom-0 transition-transform" :style="{ transform: `translateX(${humanTranslateX}px) scaleX(${humanScaleX}%)` }" />
      <img ref="humanRef" src="/graphics/martian.svg" class="w-8 absolute left-0 bottom-0 transition-transform" :style="{ transform: `translateX(${martianTranslateX}px)` }" />
    </div> 
  </div>
</template>
