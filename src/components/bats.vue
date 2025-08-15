<script setup>
import { ref } from "vue";

/* Generate a handful of bats with random paths */
const N = 12;
const bats = ref(
  Array.from({ length: N }, () => ({
    top: 10 + Math.random() * 75,          // vh
    scale: 0.6 + Math.random() * 0.8,      // size
    duration: 12 + Math.random() * 10,     // seconds
    delay: Math.random() * 8,              // seconds
    flip: Math.random() < 0.5              // left/right
  }))
);
</script>

<template>
  <div class="bats">
    <figure
      v-for="(b, i) in bats"
      :key="i"
      class="bat"
      :style="{
        '--top': b.top + 'vh',
        '--dur': b.duration + 's',
        '--delay': b.delay + 's',
      }"
    >
      <svg
        class="bat-shape"
        :class="{ flip: b.flip }"
        :style="{ '--scale': b.scale }"
        viewBox="0 0 64 32"
        aria-hidden="true"
      >
        <!-- simple bat silhouette -->
        <path
          d="M2,16 Q10,2 22,12 Q26,0 32,0 Q38,0 42,12 Q54,2 62,16 Q54,14 48,20 Q40,16 32,16 Q24,16 16,20 Q10,14 2,16 Z"
          fill="currentColor"
        />
      </svg>
    </figure>
  </div>
</template>

<style scoped>
.bats {
  position: fixed; inset: 0;
  pointer-events: none;
  overflow: hidden;
  z-index: 5; /* above background, below your UI */
}

.bat {
  position: absolute;
  top: var(--top);
  left: -12vw;
  animation: fly var(--dur) linear var(--delay) infinite;
  opacity: 0.9;
  filter: drop-shadow(0 1px 2px rgba(0,0,0,0.5));
}

.bat-shape {
  width: 64px; height: 32px; color: rgba(0,0,0,0.88);
  transform: scale(var(--scale));
}
.bat-shape.flip { transform: scale(var(--scale)) scaleX(-1); }

@keyframes fly {
  0%   { transform: translateX(0) translateY(0); }
  50%  { transform: translateX(55vw) translateY(-6px); }
  100% { transform: translateX(112vw) translateY(0); }
}
</style>
