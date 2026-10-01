<template>
  <div class="calendar-wrapper">
    <button class="cal-btn" @click="showMenu = !showMenu">
      <svg class="cal-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <rect x="3" y="4" width="18" height="18" rx="2" ry="2"/>
        <line x1="16" y1="2" x2="16" y2="6"/>
        <line x1="8" y1="2" x2="8" y2="6"/>
        <line x1="3" y1="10" x2="21" y2="10"/>
      </svg>
      <span>Agendar en Calendario</span>
      <svg class="chevron-icon" :class="{ rotated: showMenu }" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <polyline points="6 9 12 15 18 9"></polyline>
      </svg>
    </button>

    <Transition name="slide">
      <div v-if="showMenu" class="cal-dropdown">
        <a :href="googleCalUrl" target="_blank" rel="noopener noreferrer" class="dropdown-item">
          <span class="item-icon">🗓️</span>
          <span>Google Calendar</span>
        </a>
        <button type="button" @click="downloadIcs" class="dropdown-item">
          <span class="item-icon">🍎</span>
          <span>Apple / Outlook (.ics)</span>
        </button>
      </div>
    </Transition>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const showMenu = ref(false);

const eventDetails = {
  title: "Cumpleaños 🎂 - IT'S MY BIRTHDAY",
  description: "¡Acompáñame a cantar mi cumpleaños! Sábado 03 de Octubre a las 5:30PM en Mi casa (Ubicación: https://maps.app.goo.gl/2YRbVZ77zuzh4L7CA).",
  location: "Mi casa, https://maps.app.goo.gl/2YRbVZ77zuzh4L7CA",
  startDate: "20261003T173000",
  endDate: "20261003T233000"
};

const googleCalUrl = `https://calendar.google.com/calendar/render?action=TEMPLATE&text=${encodeURIComponent(
  eventDetails.title
)}&dates=${eventDetails.startDate}/${eventDetails.endDate}&details=${encodeURIComponent(
  eventDetails.description
)}&location=${encodeURIComponent(eventDetails.location)}`;

const downloadIcs = () => {
  const icsContent = [
    "BEGIN:VCALENDAR",
    "VERSION:2.0",
    "PRODID:-//Birthday Invitation//ES",
    "BEGIN:VEVENT",
    `SUMMARY:${eventDetails.title}`,
    `DESCRIPTION:${eventDetails.description}`,
    `LOCATION:${eventDetails.location}`,
    `DTSTART:${eventDetails.startDate}`,
    `DTEND:${eventDetails.endDate}`,
    "STATUS:CONFIRMED",
    "END:VEVENT",
    "END:VCALENDAR"
  ].join("\r\n");

  const blob = new Blob([icsContent], { type: "text/calendar;charset=utf-8" });
  const url = URL.createObjectURL(blob);
  const link = document.createElement("a");
  link.href = url;
  link.setAttribute("download", "cumpleanos.ics");
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  URL.revokeObjectURL(url);
  showMenu.value = false;
};
</script>

<style scoped>
.calendar-wrapper {
  position: relative;
  width: 100%;
}

.cal-btn {
  width: 100%;
  padding: 13px 18px;
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border: 1px solid rgba(0, 0, 0, 0.12);
  border-radius: 14px;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.85rem;
  font-weight: 600;
  color: #222222;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  transition: all 0.2s ease;
}

.cal-btn:hover {
  background: #ffffff;
  border-color: rgba(212, 175, 55, 0.6);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.08);
}

.cal-icon {
  width: 18px;
  height: 18px;
  color: #aa820a;
}

.chevron-icon {
  width: 16px;
  height: 16px;
  margin-left: 2px;
  transition: transform 0.2s;
}

.chevron-icon.rotated {
  transform: rotate(180deg);
}

.cal-dropdown {
  position: absolute;
  top: calc(100% + 6px);
  left: 0;
  right: 0;
  background: #ffffff;
  border-radius: 14px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
  border: 1px solid rgba(0, 0, 0, 0.08);
  overflow: hidden;
  z-index: 50;
  display: flex;
  flex-direction: column;
}

.dropdown-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px 16px;
  font-family: 'Montserrat', sans-serif;
  font-size: 0.85rem;
  font-weight: 500;
  color: #333333;
  text-decoration: none;
  background: none;
  border: none;
  width: 100%;
  cursor: pointer;
  text-align: left;
  transition: background-color 0.15s;
}

.dropdown-item:hover {
  background-color: #f7f8fa;
  color: #aa820a;
}

.dropdown-item:not(:last-child) {
  border-bottom: 1px solid #f0f0f2;
}

.item-icon {
  font-size: 1.1rem;
}

.slide-enter-active,
.slide-leave-active {
  transition: all 0.2s ease;
}

.slide-enter-from,
.slide-leave-to {
  opacity: 0;
  transform: translateY(-8px);
}
</style>
