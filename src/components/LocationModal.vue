<template>
  <div>
    <!-- Interactive Location Trigger (Styled as the Pin + "Mi casa" from the image) -->
    <div 
      class="location-trigger" 
      role="button" 
      tabindex="0" 
      @click="isOpen = true"
      @keydown.enter="isOpen = true"
      title="Toca para ver cómo llegar"
    >
      <div class="pin-icon-wrap">
        <svg class="location-pin" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
          <path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7z" stroke-linecap="round" stroke-linejoin="round"/>
          <circle cx="12" cy="9" r="2.8" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
      <span class="location-text">Mi casa</span>
    </div>

    <!-- Location Modal Backdrop -->
    <Transition name="fade">
        <div v-if="isOpen" class="loc-modal-backdrop" @click.self="isOpen = false">
          <div class="loc-modal-card">
            <button class="loc-close-btn" @click="isOpen = false" aria-label="Cerrar">✕</button>

            <div class="loc-icon-bubble">
              <svg viewBox="0 0 24 24" class="bubble-pin" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7z" stroke-linecap="round" stroke-linejoin="round"/>
                <circle cx="12" cy="9" r="3" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </div>

            <h3 class="loc-title">Ubicación del Evento</h3>
            <p class="loc-place-name">Mi casa</p>
            <p class="loc-desc">
              Toca para abrir la ubicación con la aplicación de mapas que prefieras:
            </p>

            <div class="map-actions">
              <a 
                :href="locationUrl" 
                target="_blank" 
                rel="noopener noreferrer" 
                class="map-btn open-location"
              >
                <span class="btn-emoji">📍</span>
                <span>Abrir Ubicación</span>
              </a>
            </div>
          </div>
        </div>
      </Transition>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const isOpen = ref(false);

// Real event location
const locationUrl = 'https://maps.app.goo.gl/2YRbVZ77zuzh4L7CA';
</script>

<style scoped>
.location-trigger {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  cursor: pointer;
  padding: 4px 12px;
  border-radius: 99px;
  transition: all 0.25s ease;
  user-select: none;
  background: transparent;
}

.location-trigger:hover {
  background: rgba(255, 255, 255, 0.6);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
  transform: translateY(-1px);
}

.pin-icon-wrap {
  display: flex;
  align-items: center;
  justify-content: center;
}

.location-pin {
  width: 26px;
  height: 26px;
  color: #111111;
  stroke: #111111;
}

.location-text {
  font-family: 'Playfair Display', 'Bodoni Moda', serif;
  font-size: clamp(1.8rem, 8vw, 2.3rem);
  color: #111111;
  letter-spacing: 0.01em;
  line-height: 1;
}

/* Modal Styling */
.loc-modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(12, 15, 20, 0.82);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  padding: 20px;
}

.loc-modal-card {
  position: relative;
  width: 100%;
  max-width: 380px;
  background: #ffffff;
  border-radius: 24px;
  padding: 28px 22px;
  text-align: center;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.35);
  animation: popIn 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.loc-close-btn {
  position: absolute;
  top: 14px;
  right: 14px;
  background: #f1f3f6;
  border: none;
  border-radius: 50%;
  width: 32px;
  height: 32px;
  font-size: 16px;
  color: #555;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.loc-icon-bubble {
  width: 58px;
  height: 58px;
  border-radius: 50%;
  background: #fdfaf0;
  border: 1px solid rgba(212, 175, 55, 0.35);
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 16px;
}

.bubble-pin {
  width: 30px;
  height: 30px;
  color: #aa820a;
}

.loc-title {
  font-family: 'Bodoni Moda', serif;
  font-size: 1.4rem;
  color: #1a1a1a;
  margin-bottom: 4px;
}

.loc-place-name {
  font-family: 'Playfair Display', serif;
  font-size: 1.25rem;
  font-weight: 700;
  color: #aa820a;
  margin-bottom: 12px;
}

.loc-desc {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.85rem;
  color: #666666;
  line-height: 1.45;
  margin-bottom: 22px;
}

.map-actions {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.map-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 13px;
  border-radius: 12px;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  text-decoration: none;
  transition: transform 0.2s, box-shadow 0.2s;
}

.map-btn.open-location {
  background: linear-gradient(135deg, #1f2229 0%, #0d0e12 100%);
  color: #f7e28b;
  border: 1px solid rgba(212, 175, 55, 0.7);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.25), 0 0 10px rgba(212, 175, 55, 0.2);
  font-weight: 700;
  letter-spacing: 0.04em;
}

.map-btn.open-location:hover {
  background: #111318;
  border-color: #f7e28b;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.35), 0 0 15px rgba(212, 175, 55, 0.4);
}

.map-btn:hover {
  transform: translateY(-2px);
}

@keyframes popIn {
  from { opacity: 0; transform: scale(0.92); }
  to { opacity: 1; transform: scale(1); }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
