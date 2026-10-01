<template>
  <div class="sparkles-container" aria-hidden="true">
    <div
      v-for="sparkle in sparkles"
      :key="sparkle.id"
      class="sparkle-item"
      :style="{
        top: sparkle.top,
        left: sparkle.left,
        animationDelay: sparkle.delay,
        animationDuration: sparkle.duration,
        transform: `scale(${sparkle.scale})`
      }"
    >
      <svg viewBox="0 0 24 24" class="sparkle-svg" :class="sparkle.color">
        <path
          d="M12,0 L14.5,9.5 L24,12 L14.5,14.5 L12,24 L9.5,14.5 L0,12 L9.5,9.5 Z"
        />
      </svg>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

interface Sparkle {
  id: number;
  top: string;
  left: string;
  delay: string;
  duration: string;
  scale: number;
  color: 'gold' | 'silver' | 'white';
}

const sparkles = ref<Sparkle[]>([]);

onMounted(() => {
  // Sparkle positions tailored to highlight the stars and disco balls in the background
  sparkles.value = [
    // Top Gold Star
    { id: 1, top: '8%', left: '44%', delay: '0s', duration: '2.5s', scale: 0.9, color: 'gold' },
    { id: 2, top: '12%', left: '38%', delay: '0.8s', duration: '3s', scale: 0.7, color: 'gold' },
    
    // Top Right Silver Star
    { id: 3, top: '7%', left: '80%', delay: '0.4s', duration: '2.2s', scale: 0.8, color: 'silver' },
    { id: 4, top: '11%', left: '86%', delay: '1.2s', duration: '2.8s', scale: 0.6, color: 'white' },
    
    // Left Silver Sequin & Star Area
    { id: 5, top: '35%', left: '8%', delay: '0.2s', duration: '2.4s', scale: 1.0, color: 'silver' },
    { id: 6, top: '42%', left: '14%', delay: '1.5s', duration: '3.1s', scale: 0.7, color: 'white' },
    
    // Right Disco Ball
    { id: 7, top: '28%', left: '94%', delay: '0.6s', duration: '2.1s', scale: 0.8, color: 'white' },
    { id: 8, top: '32%', left: '91%', delay: '1.8s', duration: '2.6s', scale: 0.6, color: 'silver' },
    
    // Bottom Right Disco Balls & Stars
    { id: 9, top: '75%', left: '88%', delay: '0.3s', duration: '2.7s', scale: 1.0, color: 'silver' },
    { id: 10, top: '82%', left: '85%', delay: '1.1s', duration: '2.3s', scale: 0.8, color: 'white' },
    { id: 11, top: '91%', left: '90%', delay: '0.7s', duration: '3.2s', scale: 0.9, color: 'gold' },
    
    // Bottom Left Sequin & Gold Star
    { id: 12, top: '72%', left: '9%', delay: '0.5s', duration: '2.6s', scale: 0.9, color: 'gold' },
    { id: 13, top: '88%', left: '16%', delay: '1.4s', duration: '2.9s', scale: 0.7, color: 'silver' },
    
    // Bottom Silver Star
    { id: 14, top: '90%', left: '55%', delay: '0.9s', duration: '2.4s', scale: 0.9, color: 'silver' }
  ];
});
</script>

<style scoped>
.sparkles-container {
  position: absolute;
  inset: 0;
  pointer-events: none;
  overflow: hidden;
  z-index: 5;
}

.sparkle-item {
  position: absolute;
  animation: sparkleTwinkle ease-in-out infinite;
  will-change: transform, opacity;
}

.sparkle-svg {
  width: 22px;
  height: 22px;
  filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.8));
}

.sparkle-svg.gold path {
  fill: #ffd966;
}

.sparkle-svg.silver path {
  fill: #e8f0fe;
}

.sparkle-svg.white path {
  fill: #ffffff;
}

@keyframes sparkleTwinkle {
  0%, 100% {
    opacity: 0.15;
    transform: scale(0.6) rotate(0deg);
  }
  50% {
    opacity: 1;
    transform: scale(1.25) rotate(45deg);
    filter: drop-shadow(0 0 10px rgba(255, 255, 255, 1));
  }
}
</style>
