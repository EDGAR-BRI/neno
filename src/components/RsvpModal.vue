<template>
  <div class="rsvp-wrapper">
    <!-- Trigger Button -->
    <button class="rsvp-trigger-btn" @click="isOpen = true">
      <span class="btn-shimmer"></span>
      <span class="btn-icon">🍾</span>
      <span class="btn-text">Confirmar Asistencia</span>
    </button>

    <!-- Modal Backdrop -->
    <Transition name="fade">
      <div v-if="isOpen" class="modal-backdrop" @click.self="isOpen = false">
        <div class="modal-content">
            <!-- Close Button -->
            <button class="close-btn" @click="isOpen = false" aria-label="Cerrar">
              ✕
            </button>

            <!-- Modal Header -->
            <div class="modal-header">
              <span class="modal-badge">R.S.V.P.</span>
              <h2 class="modal-title">¿Nos acompañas?</h2>
              <p class="modal-subtitle">
                Por favor confirma tu asistencia para preparar todo con cariño.
              </p>
            </div>

            <!-- Form -->
            <form @submit.prevent="sendWhatsApp" class="rsvp-form">
              <!-- Campo de nombre (removido por ahora)
              <div class="form-group">
                <label for="name">Tu Nombre Completo</label>
                <input
                  id="name"
                  v-model="guestName"
                  type="text"
                  placeholder="Ej. Sofía Martínez"
                />
              </div>
              -->

              <div class="form-group">
                <label>¿Asistirás a la fiesta?</label>
                <div class="status-options">
                  <button
                    type="button"
                    class="status-btn"
                    :class="{ active: attendanceStatus === 'si' }"
                    @click="attendanceStatus = 'si'"
                  >
                    <span>🎉</span> ¡Sí, ahí estaré!
                  </button>
                  <button
                    type="button"
                    class="status-btn"
                    :class="{ active: attendanceStatus === 'no' }"
                    @click="attendanceStatus = 'no'"
                  >
                    <span>🥺</span> No podré ir
                  </button>
                </div>
              </div>

              <!-- Campo de acompañantes (comentado por ahora)
              <div v-if="attendanceStatus === 'si'" class="form-group">
                <label for="guests">Número de acompañantes</label>
                <select id="guests" v-model="companions">
                  <option :value="0">Solo yo (1 persona)</option>
                  <option :value="1">+1 acompañante (2 personas)</option>
                  <option :value="2">+2 acompañantes (3 personas)</option>
                  <option :value="3">+3 o más personas</option>
                </select>
              </div>
              -->

              <div class="form-group">
                <label for="message">Mensaje especial (opcional)</label>
                <textarea
                  id="message"
                  v-model="customMessage"
                  rows="2"
                  placeholder="¡Felicidades! Tengo muchas ganas de celebrar contigo..."
                ></textarea>
              </div>

              <button type="submit" class="submit-whatsapp-btn">
                <svg viewBox="0 0 24 24" class="wa-icon">
                  <path
                    fill="currentColor"
                    d="M12.04 2c-5.46 0-9.91 4.45-9.91 9.91 0 1.75.46 3.45 1.32 4.95L2.05 22l5.25-1.38c1.45.79 3.08 1.21 4.74 1.21 5.46 0 9.91-4.45 9.91-9.91 0-2.65-1.03-5.14-2.9-7.01A9.816 9.816 0 0 0 12.04 2m.01 1.67c4.54 0 8.24 3.7 8.24 8.24 0 2.2-.86 4.28-2.42 5.84-1.56 1.56-3.64 2.42-5.83 2.42-1.46 0-2.88-.39-4.13-1.13l-.3-.18-3.08.81.82-3-.2-.31c-.81-1.3-1.24-2.81-1.24-4.45 0-4.54 3.7-8.24 8.24-8.24m4.52 11.59c-.25-.13-1.47-.72-1.7-.81-.23-.08-.39-.13-.56.13-.17.25-.64.81-.79.97-.14.17-.29.19-.54.06-.25-.13-1.06-.39-2.02-1.25-.75-.67-1.26-1.5-1.4-1.75-.15-.25-.02-.39.11-.51.11-.11.25-.29.38-.44.12-.14.17-.25.25-.42.08-.17.04-.31-.02-.44-.06-.13-.56-1.35-.77-1.85-.2-.49-.41-.42-.56-.43h-.48c-.17 0-.44.06-.67.31-.23.25-.88.86-.88 2.1 0 1.24.9 2.44 1.03 2.61.13.17 1.78 2.72 4.31 3.81.6.26 1.07.42 1.44.54.61.19 1.16.17 1.6.1.49-.07 1.47-.6 1.68-1.18.21-.59.21-1.09.14-1.19-.06-.1-.22-.17-.47-.29"
                  />
                </svg>
                Confirmar por WhatsApp
              </button>
            </form>
          </div>
        </div>
      </Transition>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const isOpen = ref(false);
const guestName = ref('');
const attendanceStatus = ref<'si' | 'no'>('si');
const companions = ref(0);
const customMessage = ref('');

const sendWhatsApp = () => {
  const isGoing = attendanceStatus.value === 'si';
  let text = `🎉 *CONFIRMACIÓN DE ASISTENCIA*\n\n`;
  text += `✨ *Respuesta:* ${isGoing ? '¡Hola! Confirmo con mucho gusto mi asistencia a tu cumpleaños 🥳' : '¡Hola! Lamentablemente no podré asistir esta vez 🥺'}\n`;

  if (customMessage.value.trim()) {
    text += `💌 *Mensaje:* ${customMessage.value.trim()}\n`;
  }

  text += `\n📅 *Evento:* Cumpleaños - Sábado 03 de Octubre a las 5:30PM\n📍 *Ubicación:* https://maps.app.goo.gl/2YRbVZ77zuzh4L7CA`;

  // WhatsApp number: +584125129709
  const waUrl = `https://api.whatsapp.com/send?phone=584125129709&text=${encodeURIComponent(text)}`;
  window.open(waUrl, '_blank');
  isOpen.value = false;
};
</script>

<style scoped>
.rsvp-wrapper {
  width: 100%;
}

.rsvp-trigger-btn {
  position: relative;
  width: 100%;
  padding: 16px 24px;
  background: linear-gradient(135deg, #1f2229 0%, #0d0e12 100%);
  color: #f7e28b;
  border: 1px solid rgba(212, 175, 55, 0.7);
  border-radius: 999px;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.95rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  cursor: pointer;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.35), 0 0 15px rgba(212, 175, 55, 0.25);
  overflow: hidden;
  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
}

.rsvp-trigger-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.45), 0 0 25px rgba(212, 175, 55, 0.45);
  border-color: #f7e28b;
}

.rsvp-trigger-btn:active {
  transform: translateY(1px);
}

.btn-icon {
  font-size: 1.25rem;
}

.btn-shimmer {
  position: absolute;
  top: 0;
  left: -100%;
  width: 60%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent 0%,
    rgba(255, 255, 255, 0.25) 50%,
    transparent 100%
  );
  transform: skewX(-25deg);
  animation: btnGleam 3.5s infinite;
}

@keyframes btnGleam {
  0% { left: -100%; }
  35%, 100% { left: 160%; }
}

/* Modal styling */
.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(10, 12, 16, 0.82);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  display: flex;
  align-items: flex-end;
  justify-content: center;
  z-index: 9999;
  padding: 0;
}

@media (min-width: 480px) {
  .modal-backdrop {
    align-items: center;
    padding: 20px;
  }
}

.modal-content {
  position: relative;
  width: 100%;
  max-width: 440px;
  background: #ffffff;
  border-radius: 28px 28px 0 0;
  padding: 28px 24px 34px;
  box-shadow: 0 -10px 40px rgba(0, 0, 0, 0.4);
  max-height: 90dvh;
  overflow-y: auto;
  animation: slideUp 0.35s cubic-bezier(0.16, 1, 0.3, 1);
}

@media (min-width: 480px) {
  .modal-content {
    border-radius: 24px;
    box-shadow: 0 20px 50px rgba(0, 0, 0, 0.45);
    animation: scaleIn 0.3s cubic-bezier(0.16, 1, 0.3, 1);
  }
}

.close-btn {
  position: absolute;
  top: 16px;
  right: 18px;
  background: #f0f2f5;
  border: none;
  border-radius: 50%;
  width: 32px;
  height: 32px;
  font-size: 16px;
  color: #666;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-header {
  text-align: center;
  margin-bottom: 22px;
}

.modal-badge {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.2em;
  color: #aa820a;
  background: #fff8df;
  padding: 4px 10px;
  border-radius: 99px;
  display: inline-block;
  margin-bottom: 8px;
}

.modal-title {
  font-family: 'Bodoni Moda', serif;
  font-size: 1.6rem;
  color: #111;
  margin-bottom: 6px;
}

.modal-subtitle {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.85rem;
  color: #666;
  line-height: 1.4;
}

.rsvp-form {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
  text-align: left;
}

.form-group label {
  font-family: 'Montserrat', sans-serif;
  font-size: 0.8rem;
  font-weight: 600;
  color: #333;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 12px 14px;
  border: 1px solid #d9dce1;
  border-radius: 12px;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.9rem;
  color: #1a1a1a;
  background: #fbfbfd;
  outline: none;
  transition: border-color 0.2s, box-shadow 0.2s;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  border-color: #d4af37;
  box-shadow: 0 0 0 3px rgba(212, 175, 55, 0.15);
  background: #fff;
}

.status-options {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}

.status-btn {
  padding: 12px 8px;
  border: 1.5px solid #e2e5eb;
  border-radius: 12px;
  background: #fff;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.85rem;
  font-weight: 600;
  color: #555;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s;
}

.status-btn.active {
  border-color: #d4af37;
  background: #fdfaf0;
  color: #aa820a;
}

.submit-whatsapp-btn {
  width: 100%;
  padding: 14px;
  background: #25d366;
  color: #fff;
  border: none;
  border-radius: 14px;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.95rem;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  cursor: pointer;
  box-shadow: 0 4px 15px rgba(37, 211, 102, 0.35);
  margin-top: 6px;
  transition: transform 0.2s, background-color 0.2s;
}

.submit-whatsapp-btn:hover {
  background: #20ba59;
  transform: translateY(-1px);
}

.wa-icon {
  width: 22px;
  height: 22px;
}

/* Animations */
@keyframes slideUp {
  from { transform: translateY(100%); }
  to { transform: translateY(0); }
}

@keyframes scaleIn {
  from { transform: scale(0.92); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
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
