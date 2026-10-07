<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const steps = [
  { n: '01', title: 'Заявка', text: 'Консультация и оценка задачи.' },
  { n: '02', title: 'Договор', text: 'Фиксируем сроки и стоимость.' },
  { n: '03', title: 'Исполнение', text: 'Собираем данные, готовим документацию.' },
  { n: '04', title: 'Сдача', text: 'Формируем и сдаём отчёты точно в срок.' }
]

const base = import.meta.env.BASE_URL

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
  <section ref="root" class="about" :class="{ 'is-visible': visible }">
    <div class="dim"></div>

    <div class="frame">
      <header class="top">
        <span class="page">02</span>
        <span class="rule"></span>
        <span class="kicker">О нас · Этапы · Объекты</span>
      </header>

      <div class="body">
        <div class="left">
          <span class="kicker-small">01 · О компании</span>

          <h2 class="title">
            Честный<br />
            эколог
          </h2>

          <p class="lead">
            Объединение опытных инженеров-экологов и экологов-юристов.
            Комплексный подход: консалтинг, документация, отчётность
            и представление интересов в органах власти.
          </p>

          <div class="facts">
            <div class="fact">
              <span class="fact-n">20+</span>
              <span class="fact-t">лет опыта на опасных производственных объектах</span>
            </div>
            <div class="fact">
              <span class="fact-n">РФ</span>
              <span class="fact-t">работаем по всей России, выезжаем на места</span>
            </div>
            <div class="fact">
              <span class="fact-n">04</span>
              <span class="fact-t">направления: вода · воздух · отходы · общее</span>
            </div>
          </div>
        </div>

        <div class="right">
          <span class="kicker-small">02 · С объектов</span>

          <div class="shots">
            <div class="shot shot--a" :style="{ backgroundImage: `url(${base}1.png)` }"></div>
            <div class="shot shot--b" :style="{ backgroundImage: `url(${base}2.png)` }"></div>
            <div class="shot shot--c" :style="{ backgroundImage: `url(${base}3.png)` }"></div>
          </div>

          <p class="caption">Fig. 04 — наши сотрудники на объектах</p>
        </div>
      </div>

      <footer class="bottom">
        <div class="steps-head">
          <span class="kicker-small">03 · Этапы работ</span>
        </div>
        <ol class="steps">
          <li v-for="s in steps" :key="s.n" class="step">
            <span class="step-n">{{ s.n }}</span>
            <span class="step-t">{{ s.title }}</span>
            <span class="step-d">{{ s.text }}</span>
          </li>
        </ol>
      </footer>

      <span class="v-mark">Ecohonest · 2026</span>
    </div>
  </section>
</template>

<style scoped>
.about {
  position: relative;
  height: 100vh;
  padding: 50px 0;
  display: flex;
  flex-direction: column;
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.1) 0%,
    rgba(15, 12, 8, 0.25) 55%,
    rgba(0, 0, 0, 0.55) 100%
  );
  color: #eef1e8;
  font-family: Georgia, "Times New Roman", serif;
  overflow: hidden;
  box-sizing: border-box;
}

.dim {
  position: absolute;
  inset: 0;
  background: rgba(10, 8, 6, 0.15);
  pointer-events: none;
  z-index: 0;
}

.frame {
  position: relative;
  z-index: 1;
  flex: 1;
  padding: 24px 80px 20px;
  display: flex;
  flex-direction: column;
  gap: 26px;
  min-height: 0;
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

@keyframes title-reveal {
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

@keyframes shot-in {
  from { opacity: 0; transform: translateY(24px) scale(0.98); filter: blur(8px); }
  to   { opacity: 1; transform: translateY(0) scale(1);        filter: blur(0); }
}

@keyframes dot-pulse {
  0%, 100% {
    box-shadow: 0 0 0 0 rgba(201, 226, 101, 0.55);
    opacity: 1;
  }
  50% {
    box-shadow: 0 0 0 6px rgba(201, 226, 101, 0);
    opacity: 0.7;
  }
}

.about:not(.is-visible) .top,
.about:not(.is-visible) .kicker-small,
.about:not(.is-visible) .title,
.about:not(.is-visible) .lead,
.about:not(.is-visible) .facts,
.about:not(.is-visible) .right,
.about:not(.is-visible) .shot,
.about:not(.is-visible) .caption,
.about:not(.is-visible) .bottom,
.about:not(.is-visible) .step,
.about:not(.is-visible) .v-mark,
.about:not(.is-visible) .rule {
  opacity: 0;
}

.about.is-visible .top        { animation: fade-right 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.05s both; }
.about.is-visible .rule       { transform-origin: left center; animation: rule-grow 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.25s both; }

.about.is-visible .left .kicker-small { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.25s both; }
.about.is-visible .title              { animation: title-reveal 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.35s both; }
.about.is-visible .lead               { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.60s both; }
.about.is-visible .facts              { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.75s both; }

.about.is-visible .right > .kicker-small { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.45s both; }
.about.is-visible .shot--a               { animation: shot-in 1.0s cubic-bezier(0.22, 1, 0.36, 1) 0.55s both; }
.about.is-visible .shot--b               { animation: shot-in 1.0s cubic-bezier(0.22, 1, 0.36, 1) 0.68s both; }
.about.is-visible .shot--c               { animation: shot-in 1.0s cubic-bezier(0.22, 1, 0.36, 1) 0.81s both; }
.about.is-visible .caption               { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.95s both; }

.about.is-visible .bottom                { animation: fade-up 1.0s cubic-bezier(0.22, 1, 0.36, 1) 0.95s both; }
.about.is-visible .step:nth-child(1)     { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 1.05s both; }
.about.is-visible .step:nth-child(2)     { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 1.15s both; }
.about.is-visible .step:nth-child(3)     { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 1.25s both; }
.about.is-visible .step:nth-child(4)     { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 1.35s both; }

.about.is-visible .v-mark { animation: v-mark-in 1.4s ease-out 0.15s both; }

/* ═════════ Layout ═════════ */

.top {
  display: flex;
  align-items: center;
  gap: 16px;
  font-family: system-ui, sans-serif;
}

.page { font-size: 11px; letter-spacing: 0.2em; color: #c9e265; }
.rule { flex: 1; height: 1px; background: rgba(238, 241, 232, 0.4); }
.kicker { font-size: 10px; letter-spacing: 0.25em; text-transform: uppercase; color: rgba(238, 241, 232, 0.9); }

.kicker-small {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 0.28em;
  text-transform: uppercase;
  color: #c9e265;
}

.kicker-small::before {
  content: '';
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #c9e265;
  flex-shrink: 0;
  animation: dot-pulse 2s ease-in-out infinite;
}

.left .kicker-small::before       { animation-delay: 0s; }
.right > .kicker-small::before    { animation-delay: 0.6s; }
.steps-head .kicker-small::before { animation-delay: 1.2s; }

.body {
  flex: 1;
  display: grid;
  grid-template-columns: 1.4fr 1fr;
  gap: 80px;
  align-items: center;
  min-height: 0;
}

.left {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.title {
  font-size: clamp(56px, 6.5vw, 108px);
  font-weight: 400;
  letter-spacing: -0.045em;
  line-height: 0.9;
  color: #eef1e8;
  margin-left: -0.045em;
}

.lead {
  font-family: system-ui, sans-serif;
  font-size: 13px;
  line-height: 1.55;
  color: rgba(238, 241, 232, 0.72);
  max-width: 440px;
}

.facts {
  display: flex;
  gap: 40px;
  margin-top: 8px;
  padding-top: 18px;
  border-top: 1px solid rgba(238, 241, 232, 0.15);
}

.fact {
  display: flex;
  flex-direction: column;
  gap: 4px;
  min-width: 0;
}

.fact-n {
  font-family: Georgia, serif;
  font-size: 26px;
  letter-spacing: -0.02em;
  line-height: 1;
  color: #c9e265;
}

.fact-t {
  font-family: system-ui, sans-serif;
  font-size: 10px;
  line-height: 1.4;
  letter-spacing: 0.04em;
  color: rgba(238, 241, 232, 0.55);
  max-width: 150px;
}

/* ─── фотопоток ─── */
.right {
  display: flex;
  flex-direction: column;
  gap: 12px;
  height: 100%;
  max-height: 100%;
}

.shots {
  flex: 1;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1.4fr 1fr;
  gap: 8px;
  min-height: 0;
}

.shot {
  border-radius: 2px;
  background-size: cover;
  background-position: center;
  background-color: #1a1a1a;
  box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.06);
}

.shot--a {
  grid-column: 1 / -1;
}

.caption {
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: rgba(238, 241, 232, 0.4);
  flex-shrink: 0;
}

/* ─── этапы ─── */
.bottom {
  border-top: 1px solid rgba(238, 241, 232, 0.18);
  padding-top: 18px;
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.steps-head { display: flex; align-items: center; }

.steps {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 40px;
  list-style: none;
  margin: 0;
  padding: 0;
}

.step {
  display: flex;
  flex-direction: column;
  gap: 6px;
  font-family: system-ui, sans-serif;
  position: relative;
  padding-left: 22px;
}

.step::before {
  content: '';
  position: absolute;
  left: 0;
  top: 6px;
  width: 8px;
  height: 1px;
  background: #c9e265;
}

.step-n { font-size: 10px; letter-spacing: 0.22em; color: #c9e265; }

.step-t {
  font-size: 12px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  font-weight: 600;
  color: #eef1e8;
}

.step-d {
  font-size: 11px;
  line-height: 1.4;
  color: rgba(238, 241, 232, 0.55);
}

/* ─── водяной знак ─── */
.v-mark {
  position: absolute;
  left: 20px;
  top: 50%;
  transform: rotate(-90deg) translateX(-50%);
  transform-origin: left center;
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 0.35em;
  text-transform: uppercase;
  color: rgba(238, 241, 232, 0.4);
  white-space: nowrap;
  pointer-events: none;
}

/* ═════════ Планшет ═════════ */

@media (max-width: 1100px) {
  .body { grid-template-columns: 1fr; gap: 32px; }
  .right { height: auto; }
  .shots { aspect-ratio: 16 / 7; }
  .steps { grid-template-columns: repeat(2, 1fr); gap: 20px; }
}

/* ═════════ Мобила (портрет) ═════════ */

@media (max-width: 768px) and (orientation: portrait) {
  .about {
    height: auto;
    min-height: 100dvh;
    /* уменьшил нижний отступ — оставляем свободное место снизу */
    padding: 20px 0 16px;
    overflow: visible;
    /* усиленное затемнение сверху и снизу */
    background: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.35) 0%,
      rgba(15, 12, 8, 0.35) 45%,
      rgba(0, 0, 0, 0.72) 100%
    );
  }

  .frame {
    padding: 16px 20px 8px;
    gap: 20px;
  }

  .top { gap: 12px; }
  .page { font-size: 10px; letter-spacing: 0.18em; }
  .kicker { font-size: 9px; letter-spacing: 0.18em; }

  .body {
    display: flex;
    flex-direction: column;
    gap: 22px;
    min-height: 0;
  }

  .left  { gap: 14px; }
  .right { height: auto; gap: 10px; }

  .right > .kicker-small {
    display: inline-flex;
    font-size: 9px;
    letter-spacing: 0.24em;
  }

  /* ─── карусель ─── */
  .shots {
    display: flex;
    gap: 10px;
    overflow-x: auto;
    overflow-y: hidden;
    scroll-snap-type: x mandatory;
    scrollbar-width: none;
    -webkit-overflow-scrolling: touch;
    flex: none;
    aspect-ratio: 4 / 3;
    width: 100%;
    border-radius: 4px;
    min-height: 0;
  }

  .shots::-webkit-scrollbar { display: none; }

  .shot {
    flex: 0 0 100%;
    scroll-snap-align: center;
    border-radius: 4px;
    box-shadow: none;
  }

  .shot--a { grid-column: auto; }

  .caption {
    font-size: 9px;
    letter-spacing: 0.2em;
  }

  /* ─── текст ─── */
  .left .kicker-small {
    font-size: 9px;
    letter-spacing: 0.24em;
  }

  .title {
    font-size: clamp(40px, 13vw, 60px);
    line-height: 0.9;
    letter-spacing: -0.045em;
  }

  .lead {
    font-size: 13px;
    line-height: 1.5;
    max-width: 100%;
  }

  .facts {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
    margin-top: 4px;
    padding-top: 14px;
  }

  .fact-n { font-size: 20px; }

  .fact-t {
    font-size: 9px;
    line-height: 1.35;
    max-width: 100%;
  }

  /* ─── этапы — компактнее по вертикали ─── */
  .bottom {
    padding-top: 12px;
    gap: 10px;
  }

  .steps {
    grid-template-columns: 1fr;
    gap: 10px;
  }

  .step {
    gap: 2px;
    padding-left: 16px;
  }

  .step::before {
    top: 4px;
    width: 8px;
  }

  .step-n {
    font-size: 9px;
    letter-spacing: 0.18em;
  }
  .step-t {
    font-size: 11px;
    letter-spacing: 0.18em;
  }
  .step-d {
    font-size: 10px;
    line-height: 1.3;
  }

  .v-mark { display: none; }
}

@media (max-width: 380px) and (orientation: portrait) {
  .title { font-size: 36px; }
  .fact-n { font-size: 18px; }
  .fact-t { font-size: 8px; }

  .bottom { padding-top: 10px; gap: 8px; }
  .steps { gap: 8px; }
  .step-t { font-size: 10px; }
  .step-d { font-size: 9px; }
}

/* ═════════ Мобила (ландшафт) — уменьшаем через scale ═════════ */

@media (orientation: landscape) and (max-height: 500px) {
  .about {
    position: fixed;
    top: 0;
    left: 0;
    width: calc(100vw / 0.68);
    height: calc(100vh / 0.68);
    padding: 40px 0;
    transform: scale(0.68);
    transform-origin: top left;
    overflow: hidden;
  }

  .body {
    display: grid;
    grid-template-columns: 1.4fr 1fr;
    gap: 60px;
    align-items: center;
    min-height: 0;
  }

  .left  { gap: 20px; }
  .right { height: 100%; max-height: 100%; gap: 12px; }

  .shots {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1.4fr 1fr;
    gap: 8px;
    aspect-ratio: auto;
    flex: 1;
    overflow: visible;
    border-radius: 2px;
  }

  .shot {
    flex: auto;
    scroll-snap-align: none;
    border-radius: 2px;
  }

  .shot--a { grid-column: 1 / -1; }

  .steps {
    grid-template-columns: repeat(4, 1fr);
    gap: 40px;
  }

  .step { padding-left: 22px; }

  .v-mark { display: block; }
}

/* ═════════ Reduced motion ═════════ */

@media (prefers-reduced-motion: reduce) {
  .about:not(.is-visible) .top,
  .about:not(.is-visible) .kicker-small,
  .about:not(.is-visible) .title,
  .about:not(.is-visible) .lead,
  .about:not(.is-visible) .facts,
  .about:not(.is-visible) .right,
  .about:not(.is-visible) .shot,
  .about:not(.is-visible) .caption,
  .about:not(.is-visible) .bottom,
  .about:not(.is-visible) .step,
  .about:not(.is-visible) .v-mark,
  .about:not(.is-visible) .rule {
    opacity: 1;
  }
  .about.is-visible *,
  .kicker-small::before {
    animation: none !important;
  }
}
</style>