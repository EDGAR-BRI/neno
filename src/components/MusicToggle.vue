<template>
  <div class="music-toggle-container">
    <button 
      class="music-btn" 
      :class="{ playing: isPlaying }" 
      @click="toggleAudio"
      title="Música de fiesta"
    >
      <span class="music-icon">{{ isPlaying ? '🔊' : '🎵' }}</span>
      <span class="music-label">{{ isPlaying ? 'Pausar Música' : 'Música de Fiesta' }}</span>
      <span v-if="isPlaying" class="equalizer">
        <span class="bar bar-1"></span>
        <span class="bar bar-2"></span>
        <span class="bar bar-3"></span>
      </span>
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const isPlaying = ref(false);
let audioContext: AudioContext | null = null;
let oscillator: OscillatorNode | null = null;
let gainNode: GainNode | null = null;
let isMelodyPlaying = false;
let melodyTimeout: number | undefined;

// Cheerful festive synth melody using Web Audio API (zero external asset dependencies, works instantly anywhere!)
const playFestiveMelody = () => {
  if (!isPlaying.value) return;

  try {
    const ctx = new (window.AudioContext || (window as unknown as { webkitAudioContext: typeof AudioContext }).webkitAudioContext)();
    audioContext = ctx;

    // Celebratory Happy Birthday motif notes frequencies: C4, C4, D4, C4, F4, E4
    const notes = [
      { freq: 261.63, dur: 0.35 },
      { freq: 261.63, dur: 0.35 },
      { freq: 293.66, dur: 0.7 },
      { freq: 261.63, dur: 0.7 },
      { freq: 349.23, dur: 0.7 },
      { freq: 329.63, dur: 1.2 },
      { freq: 0, dur: 0.3 }, // rest
      { freq: 261.63, dur: 0.35 },
      { freq: 261.63, dur: 0.35 },
      { freq: 293.66, dur: 0.7 },
      { freq: 261.63, dur: 0.7 },
      { freq: 392.00, dur: 0.7 },
      { freq: 349.23, dur: 1.3 }
    ];

    let noteIndex = 0;

    const playNext = () => {
      if (!isPlaying.value) return;

      const current = notes[noteIndex];
      noteIndex = (noteIndex + 1) % notes.length;

      if (current.freq > 0) {
        const osc = ctx.createOscillator();
        const gain = ctx.createGain();

        // Warm chime / glockenspiel-like tone
        osc.type = 'triangle';
        osc.frequency.setValueAtTime(current.freq, ctx.currentTime);

        gain.gain.setValueAtTime(0.001, ctx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.15, ctx.currentTime + 0.05);
        gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + current.dur);

        osc.connect(gain);
        gain.connect(ctx.destination);

        osc.start();
        osc.stop(ctx.currentTime + current.dur);
      }

      melodyTimeout = window.setTimeout(playNext, current.dur * 1000);
    };

    playNext();
  } catch (e) {
    console.error("Audio API error:", e);
  }
};

const stopAudio = () => {
  if (melodyTimeout) clearTimeout(melodyTimeout);
  if (audioContext && audioContext.state !== 'closed') {
    audioContext.close();
    audioContext = null;
  }
};

const toggleAudio = () => {
  isPlaying.value = !isPlaying.value;
  if (isPlaying.value) {
    playFestiveMelody();
  } else {
    stopAudio();
  }
};
</script>

<style scoped>
.music-toggle-container {
  display: flex;
  justify-content: center;
}

.music-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: rgba(255, 255, 255, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.25);
  border-radius: 99px;
  color: #f7e28b;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.78rem;
  font-weight: 600;
  cursor: pointer;
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  transition: all 0.25s ease;
}

.music-btn:hover {
  background: rgba(255, 255, 255, 0.25);
  border-color: #f7e28b;
  transform: translateY(-1px);
}

.music-btn.playing {
  background: rgba(212, 175, 55, 0.25);
  border-color: #d4af37;
  color: #fff;
}

.music-icon {
  font-size: 0.95rem;
}

.equalizer {
  display: inline-flex;
  align-items: flex-end;
  gap: 2px;
  height: 12px;
  margin-left: 2px;
}

.bar {
  width: 2.5px;
  background-color: #f7e28b;
  border-radius: 2px;
  animation: equalize 0.8s ease-in-out infinite alternate;
}

.bar-1 { height: 60%; animation-delay: 0.1s; }
.bar-2 { height: 100%; animation-delay: 0.3s; }
.bar-3 { height: 40%; animation-delay: 0.2s; }

@keyframes equalize {
  from { height: 20%; }
  to { height: 100%; }
}
</style>
