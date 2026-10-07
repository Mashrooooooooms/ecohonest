<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const root = ref(null)
const visible = ref(false)
let observer

onMounted(() => {
  observer = new IntersectionObserver(([entry]) => {
    if (entry.isIntersecting) {
      visible.value = true
      observer.disconnect()
    }
  }, { threshold: 0.2 })
  observer.observe(root.value)
})

onBeforeUnmount(() => observer?.disconnect())
</script>

<template>
  <section ref="root" class="contact" :class="{ 'is-visible': visible }">
    <div class="bg"></div>
    <div class="veil"></div>

    <header class="top">
      <span class="pg">06</span>
      <span class="rule"></span>
      <span class="kicker">Связь · Адрес · Часы</span>
    </header>

    <main class="stage">
      <p class="mark">Связаться</p>

      <a class="mail" href="mailto:info@ecohonest.ru">info@ecohonest.ru</a>

      <a class="phone" href="tel:+79915917778">
        +7&nbsp;991&nbsp;591&nbsp;77&nbsp;78
      </a>

      <p class="place">Москва — ул. Марии Поливановой, 9</p>
    </main>

    <span class="v-mark">Ecohonest · 2026</span>
  </section>
</template>

<style scoped>
.contact {
  --fg:     #f2f6fd;
  --muted:  rgba(242, 246, 253, 0.55);
  --hair:   rgba(242, 246, 253, 0.2);
  --accent: #5b8dff;
  --bg:     #070e1a;

  position: relative;
  height: 100vh;
  height: 100dvh;
  display: grid;
  grid-template-rows: 64px 1fr;
  color: var(--fg);
  background: var(--bg);
  font-family: Georgia, "Times New Roman", serif;
  overflow: hidden;
  isolation: isolate;
}

/* фон */
.bg {
  position: absolute;
  inset: 0;
  z-index: -2;
  background: url('/bg3.jpg') center / cover no-repeat;
  filter: saturate(1.06) brightness(0.72);
  transform: scale(1.06);
  animation: zoom 24s ease-out both;
}
@keyframes zoom {
  from { transform: scale(1.14); }
  to   { transform: scale(1.02); }
}

.veil {
  position: absolute;
  inset: 0;
  z-index: -1;
  pointer-events: none;
  background:
    radial-gradient(80% 70% at 50% 55%,
      rgba(7, 14, 26, 0.78) 0%,
      rgba(7, 14, 26, 0.5) 55%,
      rgba(7, 14, 26, 0.3) 100%),
    linear-gradient(180deg,
      rgba(7, 14, 26, 0.55) 0%,
      rgba(7, 14, 26, 0.15) 40%,
      rgba(7, 14, 26, 0.55) 100%);
}

/* ─── шапка в общем стиле — без border-bottom ─── */
.top {
  position: relative;
  z-index: 2;
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 0 40px;
  font-family: system-ui, -apple-system, sans-serif;
  text-shadow: 0 1px 12px rgba(0, 0, 0, 0.85);
}

.pg {
  font-size: 11px;
  letter-spacing: 0.2em;
  color: var(--accent);
}

.rule {
  flex: 1;
  height: 1px;
  background: rgba(242, 246, 253, 0.4);
  transform-origin: left center;
}

.kicker {
  font-size: 10px;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(242, 246, 253, 0.9);
}

/* ─── сцена ─── */
.stage {
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 0 clamp(40px, 9vw, 140px);
  gap: 26px;
  max-width: 1500px;
  margin: 0 auto;
  width: 100%;
}

.mark {
  margin: 0;
  font-family: system-ui, -apple-system, sans-serif;
  font-size: 11px;
  letter-spacing: 0.5em;
  text-transform: uppercase;
  color: var(--accent);
  text-shadow: 0 0 18px rgba(91, 141, 255, 0.6);
}

.mail {
  color: var(--fg);
  text-decoration: none;
  font-family: Georgia, serif;
  font-size: clamp(40px, 7vw, 120px);
  line-height: 1;
  letter-spacing: -0.045em;
  width: fit-content;
  position: relative;
  text-shadow:
    0 1px 2px rgba(0, 0, 0, 0.9),
    0 8px 60px rgba(0, 0, 0, 0.85),
    0 20px 120px rgba(0, 0, 0, 0.75);
  transition: color 0.5s cubic-bezier(.2, .9, .25, 1);
}
.mail:hover { color: var(--accent); }
.mail::after {
  content: "";
  position: absolute;
  left: 0;
  right: 0.06em;
  bottom: -0.06em;
  height: 2px;
  background: var(--accent);
  transform: scaleX(0);
  transform-origin: left center;
  transition: transform 0.7s cubic-bezier(.2, .9, .25, 1);
}
.mail:hover::after { transform: scaleX(1); }

.phone {
  display: inline-block;
  color: var(--fg);
  text-decoration: none;
  font-family: Georgia, "Times New Roman", serif;
  font-size: clamp(34px, 5.6vw, 96px);
  font-weight: 400;
  line-height: 1.05;
  letter-spacing: -0.02em;
  padding-right: 0.12em;
  width: fit-content;
  text-shadow:
    0 1px 2px rgba(0, 0, 0, 0.9),
    0 6px 40px rgba(0, 0, 0, 0.85),
    0 16px 90px rgba(0, 0, 0, 0.75);
  transition: color 0.4s ease;
}
.phone:hover { color: var(--accent); }

.place {
  margin: 6px 0 0;
  font-family: system-ui, -apple-system, sans-serif;
  font-size: 11px;
  letter-spacing: 0.32em;
  text-transform: uppercase;
  color: var(--muted);
  text-shadow: 0 1px 14px rgba(0, 0, 0, 0.9);
}

/* ─── водяной знак ─── */
.v-mark {
  position: absolute;
  left: 20px;
  top: 50%;
  transform: rotate(-90deg) translateX(-50%);
  transform-origin: left center;
  z-index: 2;
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 0.35em;
  text-transform: uppercase;
  color: rgba(242, 246, 253, 0.4);
  white-space: nowrap;
  pointer-events: none;
}

/* ═════════ Entrance animations ═════════ */

@keyframes fade-up {
  from { opacity: 0; transform: translateY(18px); filter: blur(6px); }
  to   { opacity: 1; transform: translateY(0);    filter: blur(0); }
}

@keyframes fade-right {
  from { opacity: 0; transform: translateX(-18px); filter: blur(6px); }
  to   { opacity: 1; transform: translateX(0);     filter: blur(0); }
}

@keyframes reveal-lg {
  from { opacity: 0; transform: translateY(32px); filter: blur(14px); }
  to   { opacity: 1; transform: translateY(0);    filter: blur(0); }
}

@keyframes rule-grow {
  from { transform: scaleX(0); }
  to   { transform: scaleX(1); }
}

@keyframes v-mark-in {
  from { opacity: 0; }
  to   { opacity: 1; }
}

.contact:not(.is-visible) .top,
.contact:not(.is-visible) .rule,
.contact:not(.is-visible) .kicker,
.contact:not(.is-visible) .mark,
.contact:not(.is-visible) .mail,
.contact:not(.is-visible) .phone,
.contact:not(.is-visible) .place,
.contact:not(.is-visible) .v-mark {
  opacity: 0;
}

.contact.is-visible .top    { animation: fade-right 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.05s both; }
.contact.is-visible .rule   { animation: rule-grow 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.25s both; }
.contact.is-visible .kicker { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.15s both; }

.contact.is-visible .mark   { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.35s both; }
.contact.is-visible .mail   { animation: reveal-lg 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.45s both; }
.contact.is-visible .phone  { animation: reveal-lg 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.65s both; }
.contact.is-visible .place  { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.90s both; }

.contact.is-visible .v-mark { animation: v-mark-in 1.4s ease-out 0.15s both; }

/* ─── адаптив ─── */
@media (max-width: 900px) {
  .stage { padding: 0 28px; gap: 22px; }
  .top   { padding: 0 28px; }

  .mail  { font-size: clamp(32px, 9vw, 64px); }
  .phone { font-size: clamp(30px, 8vw, 56px); }
}

@media (max-width: 560px) {
  .stage { padding: 0 20px; gap: 18px; }
  .top   { padding: 0 20px; gap: 12px; }
  .pg, .kicker { font-size: 9px; }
  .kicker { letter-spacing: 0.2em; }

  .mark  { font-size: 10px; letter-spacing: 0.4em; }
  .mail  { font-size: 30px; letter-spacing: -0.04em; }
  .phone { font-size: 28px; letter-spacing: -0.01em; padding-right: 0.15em; }
  .place { letter-spacing: 0.24em; }

  .v-mark { display: none; }
}

@media (prefers-reduced-motion: reduce) {
  .bg,
  .contact.is-visible * {
    animation: none !important;
    transition: none !important;
  }
  .contact:not(.is-visible) .top,
  .contact:not(.is-visible) .rule,
  .contact:not(.is-visible) .kicker,
  .contact:not(.is-visible) .mark,
  .contact:not(.is-visible) .mail,
  .contact:not(.is-visible) .phone,
  .contact:not(.is-visible) .place,
  .contact:not(.is-visible) .v-mark {
    opacity: 1;
  }
}
</style>