<template>
  <div class="countdown-card">
    <div class="countdown-header">
      <span class="sparkle-icon">✨</span>
      <span class="countdown-title">FALTAN PARA LA FIESTA</span>
      <span class="sparkle-icon">✨</span>
    </div>

    <div class="countdown-grid">
      <div class="time-block">
        <div class="time-val">{{ formatNumber(timeLeft.days) }}</div>
        <div class="time-label">Días</div>
      </div>
      <div class="time-separator">:</div>
      <div class="time-block">
        <div class="time-val">{{ formatNumber(timeLeft.hours) }}</div>
        <div class="time-label">Horas</div>
      </div>
      <div class="time-separator">:</div>
      <div class="time-block">
        <div class="time-val">{{ formatNumber(timeLeft.minutes) }}</div>
        <div class="time-label">Min</div>
      </div>
      <div class="time-separator">:</div>
      <div class="time-block">
        <div class="time-val">{{ formatNumber(timeLeft.seconds) }}</div>
        <div class="time-label">Seg</div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

interface TimeLeft {
  days: number;
  hours: number;
  minutes: number;
  seconds: number;
}

const timeLeft = ref<TimeLeft>({ days: 0, hours: 0, minutes: 0, seconds: 0 });
let timerInterval: number | undefined;

// Determine target date: Next upcoming October 3rd at 17:30 (5:30 PM)
const getTargetDate = (): Date => {
  const now = new Date();
  let targetYear = now.getFullYear();
  let target = new Date(targetYear, 9, 3, 17, 30, 0); // October is month index 9
  
  // If target already passed this year, set to next year
  if (target.getTime() < now.getTime()) {
    target = new Date(targetYear + 1, 9, 3, 17, 30, 0);
  }
  return target;
};

const updateCountdown = () => {
  const target = getTargetDate();
  const now = new Date();
  const difference = target.getTime() - now.getTime();

  if (difference <= 0) {
    timeLeft.value = { days: 0, hours: 0, minutes: 0, seconds: 0 };
    return;
  }

  const days = Math.floor(difference / (1000 * 60 * 60 * 24));
  const hours = Math.floor((difference / (1000 * 60 * 60)) % 24);
  const minutes = Math.floor((difference / 1000 / 60) % 60);
  const seconds = Math.floor((difference / 1000) % 60);

  timeLeft.value = { days, hours, minutes, seconds };
};

const formatNumber = (num: number): string => {
  return num < 10 ? `0${num}` : `${num}`;
};

onMounted(() => {
  updateCountdown();
  timerInterval = window.setInterval(updateCountdown, 1000);
});

onUnmounted(() => {
  if (timerInterval) clearInterval(timerInterval);
});
</script>

<style scoped>
.countdown-card {
  width: 100%;
  padding: 16px 20px;
  background: rgba(255, 255, 255, 0.88);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-radius: 20px;
  border: 1px solid rgba(212, 175, 55, 0.35);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.12), inset 0 1px 2px rgba(255, 255, 255, 0.9);
  text-align: center;
  transition: transform 0.3s ease;
}

.countdown-header {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-bottom: 12px;
}

.sparkle-icon {
  font-size: 0.9rem;
}

.countdown-title {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.18em;
  color: #555555;
  text-transform: uppercase;
}

.countdown-grid {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
}

.time-block {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 8px 4px;
  background: linear-gradient(180deg, #ffffff 0%, #f7f7f9 100%);
  border-radius: 12px;
  border: 1px solid rgba(0, 0, 0, 0.06);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
}

.time-val {
  font-family: 'Bodoni Moda', 'Playfair Display', serif;
  font-size: 1.6rem;
  font-weight: 700;
  color: #1a1a1a;
  line-height: 1.1;
  font-feature-settings: "tnum";
  font-variant-numeric: tabular-nums;
}

.time-label {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.65rem;
  font-weight: 500;
  color: #777777;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin-top: 3px;
}

.time-separator {
  font-family: 'Bodoni Moda', serif;
  font-size: 1.4rem;
  font-weight: 600;
  color: #888888;
  padding-bottom: 12px;
}
</style>
