<template>
  <div
    v-if="isVisible"
    class="star-intro-overlay"
    :class="{ 'fade-out': isFading }"
    @click="dismissIntro"
  >
    <svg class="theater-svg" preserveAspectRatio="xMidYMid slice" viewBox="-100 -100 200 200"><defs><linearGradient id="goldIrisGrad" x1="0%" x2="100%" y1="0%" y2="100%"><stop offset="0%" stop-color="#fff6c8"/><stop offset="50%" stop-color="#e5c158"/><stop offset="100%" stop-color="#9a7412"/></linearGradient><mask id="theaterStarHole"><path fill="#fff" d="M-400-400h800v800h-800z"/><path fill="#000" d="m0-50 14.7 30.9 32.9 3.6L23.5 7.6l5.9 32.9L0 23.5l-29.4 17 5.9-32.9-24.1-23.1 32.9-3.6Z" class="star-iris"/></mask></defs><path fill="#090a0d" d="M-400-400h800v800h-800z" mask="url(#theaterStarHole)"/><path fill="none" stroke="url(#goldIrisGrad)" stroke-width="1.8" d="m0-50 14.7 30.9 32.9 3.6L23.5 7.6l5.9 32.9L0 23.5l-29.4 17 5.9-32.9-24.1-23.1 32.9-3.6Z" class="star-iris-border" vector-effect="non-scaling-stroke"/></svg>

    <!-- Skip hint for guests -->
    <div class="tap-hint" :class="{ 'hint-fade': hasZoomStarted }">
      <span>Toca para saltar</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const isVisible = ref(true);
const isFading = ref(false);
const hasZoomStarted = ref(false);

const dismissIntro = () => {
  if (isFading.value) return;
  isFading.value = true;
  setTimeout(() => {
    isVisible.value = false;
  }, 450);
};

onMounted(() => {
  // Check if user requested replay via custom event
  window.addEventListener('replay-star-intro', () => {
    isVisible.value = true;
    isFading.value = false;
    hasZoomStarted.value = false;
    startIntroTimeline();
  });

  startIntroTimeline();
});

const startIntroTimeline = () => {
  // Step 1: Initial black with center sparkle (300ms)
  setTimeout(() => {
    hasZoomStarted.value = true;
  }, 350);

  // Step 2: Star zoom expands (around 2.1s total)
  setTimeout(() => {
    isFading.value = true;
  }, 2200);

  // Step 3: Remove from DOM completely
  setTimeout(() => {
    isVisible.value = false;
  }, 2650);
};
</script>

<style scoped>
.star-intro-overlay {
  position: fixed;
  inset: 0;
  width: 100vw;
  height: 100vh;
  z-index: 999999;
  background: transparent;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  user-select: none;
  transition: opacity 0.45s ease-out;
}

.star-intro-overlay.fade-out {
  opacity: 0;
  pointer-events: none;
}

.theater-svg {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

/* Star Iris Zoom Keyframe Animation */
.star-iris,
.star-iris-border {
  transform-origin: 0px 0px;
  animation: starTheaterZoom 2.2s cubic-bezier(0.55, 0.05, 0.2, 1) forwards;
  will-change: transform;
}

.star-iris-border {
  filter: drop-shadow(0 0 12px rgba(255, 235, 150, 0.85));
}

@keyframes starTheaterZoom {
  0% {
    transform: scale(0) rotate(0deg);
    opacity: 0.9;
  }
  15% {
    transform: scale(0.2) rotate(3deg);
    opacity: 1;
  }
  35% {
    transform: scale(0.9) rotate(8deg);
  }
  60% {
    transform: scale(3.5) rotate(14deg);
  }
  85% {
    transform: scale(15) rotate(22deg);
  }
  100% {
    transform: scale(45) rotate(28deg);
    opacity: 1;
  }
}

/* Initial Sparkle in center */
.center-sparkle {
  position: relative;
  z-index: 10;
  font-size: 2.2rem;
  color: #f7e28b;
  animation: sparklePulse 0.8s ease-in-out infinite alternate;
  transition: opacity 0.3s ease;
  filter: drop-shadow(0 0 15px rgba(247, 226, 139, 0.9));
}

.center-sparkle.sparkle-fade {
  opacity: 0;
  transform: scale(1.5);
}

@keyframes sparklePulse {
  0% { transform: scale(0.85); opacity: 0.7; }
  100% { transform: scale(1.2); opacity: 1; }
}

/* Tap to skip hint at the bottom */
.tap-hint {
  position: absolute;
  bottom: 30px;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.72rem;
  font-weight: 500;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.45);
  background: rgba(0, 0, 0, 0.4);
  padding: 6px 14px;
  border-radius: 99px;
  backdrop-filter: blur(4px);
  transition: opacity 0.4s ease;
}

.tap-hint.hint-fade {
  opacity: 0;
}
</style>
