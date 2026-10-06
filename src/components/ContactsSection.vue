<script setup>
</script>

<template>
  <section class="c">
    <div class="bg"></div>
    <div class="veil"></div>

    <header class="bar">
      <span>Ecohonest</span>
      <span>N°06</span>
      <span>MMXXVI</span>
    </header>

    <main class="stage">
      <p class="mark">Связаться</p>

      <a class="mail" href="mailto:info@ecohonest.ru">info@ecohonest.ru</a>

      <a class="phone" href="tel:+79915917778">
        +7&nbsp;991&nbsp;591&nbsp;77&nbsp;78
      </a>

      <p class="place">Москва — ул. Марии Поливановой, 9</p>
    </main>

    <footer class="bar bar--bot">
      <span>Пн — Пт · 09—19</span>
      <span>ООО "Честный эколог"</span>
      <span> - 2026 -</span>
    </footer>
  </section>
</template>

<style scoped>
.c {
  --fg:     #f2f6fd;
  --muted:  rgba(242, 246, 253, 0.55);
  --hair:   rgba(242, 246, 253, 0.2);
  --accent: #5b8dff;
  --bg:     #070e1a;

  position: relative;
  height: 100vh;
  display: grid;
  grid-template-rows: 56px 1fr 56px;
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

/* линейки */
.bar {
  position: relative;
  z-index: 2;
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  align-items: center;
  padding: 0 40px;
  font-family: system-ui, -apple-system, "Inter", sans-serif;
  font-size: 10px;
  letter-spacing: 0.34em;
  text-transform: uppercase;
  color: var(--muted);
  text-shadow: 0 1px 12px rgba(0, 0, 0, 0.9);
}
.bar > span:nth-child(1) { justify-self: start; color: var(--fg); }
.bar > span:nth-child(2) { justify-self: center; color: var(--accent); }
.bar > span:nth-child(3) { justify-self: end; }

.bar:not(.bar--bot) { border-bottom: 1px solid var(--hair); }
.bar--bot { border-top: 1px solid var(--hair); }

/* сцена */
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
  font-family: system-ui, -apple-system, "Inter", sans-serif;
  font-size: 11px;
  letter-spacing: 0.5em;
  text-transform: uppercase;
  color: var(--accent);
  text-shadow: 0 0 18px rgba(91, 141, 255, 0.6);
}

/* почта — первая, крупная */
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
  right: 0.06em; /* компенсируем минус-трекинг, линия не залезает на "m" */
  bottom: -0.06em;
  height: 2px;
  background: var(--accent);
  transform: scaleX(0);
  transform-origin: left center;
  transition: transform 0.7s cubic-bezier(.2, .9, .25, 1);
}
.mail:hover::after { transform: scaleX(1); }

/* телефон — крупный, чистый, без улёта последней цифры */
.phone {
  display: inline-block;
  color: var(--fg);
  text-decoration: none;
  font-family: Georgia, "Times New Roman", serif;
  font-size: clamp(34px, 5.6vw, 96px);
  font-weight: 400;
  line-height: 1.05;
  letter-spacing: -0.02em;
  /* ключ: место справа, чтобы последняя цифра не обрезалась */
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
  font-family: system-ui, -apple-system, "Inter", sans-serif;
  font-size: 11px;
  letter-spacing: 0.32em;
  text-transform: uppercase;
  color: var(--muted);
  text-shadow: 0 1px 14px rgba(0, 0, 0, 0.9);
}

/* адаптив */
@media (max-width: 900px) {
  .stage { padding: 0 28px; gap: 22px; }
  .bar { padding: 0 28px; font-size: 9px; letter-spacing: 0.26em; }
  .bar > span:nth-child(2) { display: none; }
  .bar { grid-template-columns: 1fr 1fr; }

  .mail  { font-size: clamp(32px, 9vw, 64px); }
  .phone { font-size: clamp(30px, 8vw, 56px); }
}

@media (max-width: 560px) {
  .stage { padding: 0 20px; gap: 18px; }
  .bar { padding: 0 20px; }
  .mark { font-size: 10px; letter-spacing: 0.4em; }
  .mail  { font-size: 30px; letter-spacing: -0.04em; }
  .phone { font-size: 28px; letter-spacing: -0.01em; padding-right: 0.15em; }
  .place { letter-spacing: 0.24em; }
}
</style>