<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

defineProps({ isActive: { type: Boolean, default: true } })

const entered = ref(false)
const root = ref(null)
let observer

onMounted(() => {
  observer = new IntersectionObserver(([entry]) => {
    if (entry.isIntersecting) {
      entered.value = true
      observer.disconnect()
    }
  }, { threshold: 0.2 })
  observer.observe(root.value)
})

onBeforeUnmount(() => observer?.disconnect())

const services = [
  { n: '01', label: 'Вода',   text: 'НДС · ЗСО · 2-ТП (водхоз) · декларация сточных вод' },
  { n: '02', label: 'Воздух', text: 'НДВ · СЗЗ · паспорт ГОУ · 2-ТП (воздух) · парниковые газы' },
  { n: '03', label: 'Отходы', text: 'ПНООЛР · лицензия · паспорта · 2-ТП (отходы) · кадастр' },
  { n: '04', label: 'Общее',  text: 'Постановка на учёт · расчёт НВОС · ПЭК · аудит · консалтинг' }
]

const dates = ['13', '14', '17', '19']
const times = ['12:30 — 13:30', '13:45 — 14:45']
const dirs = ['Вода', 'Воздух', 'Отходы', 'Общее']

const date = ref('14')
const time = ref('12:30 — 13:30')
const dir = ref('Вода')
</script>

<template>
  <section ref="root" class="services" :class="{ 'is-active': isActive || entered }">
    <div class="vignette"></div>
    <div class="dim"></div>

    <div class="frame">
      <header class="top">
        <span class="page">03</span>
        <span class="rule"></span>
        <span class="kicker">Направления · Услуги · Периметр</span>
      </header>

      <div class="body">
        <div class="left">
          <div class="phones">
            <div class="phone phone--a">
              <div class="screen">
                <div class="photo photo--a">
                  <div class="notch"></div>
                  <div class="photo-meta">
                    <span class="p-title">EcoHonest</span>
                    <span class="p-sub">Экологический аудит</span>
                  </div>
                </div>

                <div class="panel">
                  <div class="hint">Дата</div>
                  <div class="row">
                    <button
                      v-for="d in dates"
                      :key="d"
                      class="pill"
                      :class="{ 'pill--on': date === d }"
                      type="button"
                      @click="date = d"
                    >{{ d }}</button>
                  </div>

                  <div class="hint">Время</div>
                  <div class="row">
                    <button
                      v-for="t in times"
                      :key="t"
                      class="pill pill--wide"
                      :class="{ 'pill--on': time === t }"
                      type="button"
                      @click="time = t"
                    >{{ t }}</button>
                  </div>

                  <div class="foot">
                    <div>
                      <div class="hint">Стоимость</div>
                      <div class="price">от 3 000 ₽</div>
                    </div>
                    <button class="cta" type="button">Забронировать</button>
                  </div>

                  <div class="fineprint">Первая консультация — бесплатно</div>
                </div>
              </div>
            </div>

            <div class="phone phone--b">
              <div class="screen">
                <div class="photo photo--b">
                  <div class="notch"></div>
                  <div class="photo-meta">
                    <span class="p-title">Услуги</span>
                    <span class="p-sub">Полный цикл</span>
                    <span class="p-tag">Топ выбор</span>
                  </div>
                </div>

                <div class="panel">
                  <div class="hint">Направления</div>
                  <div class="grid">
                    <button
                      v-for="(d, i) in dirs"
                      :key="d"
                      class="cell"
                      :class="{ 'cell--on': dir === d }"
                      type="button"
                      @click="dir = d"
                    >
                      <span>{{ d }}</span>
                      <span class="cell-n">0{{ i + 1 }}</span>
                    </button>
                  </div>

                  <div class="author">
                    <div class="avatar"></div>
                    <div class="author-meta">
                      <span class="author-t">Сопровождение</span>
                      <span class="author-s">Команда экологов</span>
                    </div>
                    <button class="cta" type="button">Связаться</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="right">
          <span class="kicker-small">04 направления</span>

          <h2 class="title">
            Всё, что нужно<br />
            <em>производству</em>
          </h2>

          <ul class="list">
            <li v-for="s in services" :key="s.n" class="item">
              <div class="head">
                <span class="dot"></span>
                <span class="line"></span>
                <span class="label">{{ s.label }}</span>
                <span class="n">{{ s.n }}</span>
              </div>
              <p class="text">{{ s.text }}</p>
            </li>
          </ul>

          <a class="cta-big" href="tel:+79915917778">
            <span>Связаться</span>
            <span class="arrow">→</span>
          </a>
        </div>
      </div>

      <footer class="bottom">
        <span>ООО «Честный эколог» · ОГРН 1217700015158 · ИНН 9729304028</span>
        <span>119361, г. Москва, ул. Марии Поливановой, д. 9, каб. 22</span>
        <span>+7 (991) 591-77-78 · info@ecohonest.ru</span>
      </footer>

      <span class="v-mark">Ecohonest · 2026</span>
    </div>
  </section>
</template>

<style scoped>
.services {
  position: relative;
  height: 100vh;
  padding: 50px 0;
  display: flex;
  flex-direction: column;
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.15) 0%,
    rgba(40, 15, 5, 0.25) 55%,
    rgba(0, 0, 0, 0.55) 100%
  );
  color: #f5ece2;
  font-family: Georgia, "Times New Roman", serif;
  overflow: hidden;
  box-sizing: border-box;
}

/* круговое затемнение поверх фона — тёмный ореол справа, где текст */
.vignette {
  position: absolute;
  inset: 0;
  background: radial-gradient(
    ellipse 75% 95% at 72% 50%,
    rgba(15, 6, 2, 0.8) 0%,
    rgba(15, 6, 2, 0.35) 55%,
    rgba(15, 6, 2, 0.05) 100%
  );
  pointer-events: none;
  z-index: 0;
}

.dim {
  position: absolute;
  inset: 0;
  background: rgba(20, 8, 4, 0.1);
  pointer-events: none;
  z-index: 0;
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

@keyframes phone-in {
  from { opacity: 0; transform: translateY(40px) scale(0.96); filter: blur(10px); }
  to   { opacity: 1; filter: blur(0); }
}

@keyframes dot-pulse {
  0%, 100% {
    box-shadow: 0 0 0 0 rgba(232, 122, 53, 0.55);
    opacity: 1;
  }
  50% {
    box-shadow: 0 0 0 6px rgba(232, 122, 53, 0);
    opacity: 0.7;
  }
}

.services:not(.is-active) .top,
.services:not(.is-active) .rule,
.services:not(.is-active) .kicker,
.services:not(.is-active) .phones,
.services:not(.is-active) .kicker-small,
.services:not(.is-active) .title,
.services:not(.is-active) .item,
.services:not(.is-active) .cta-big,
.services:not(.is-active) .bottom,
.services:not(.is-active) .v-mark {
  opacity: 0;
}

.services.is-active .top        { animation: fade-right 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.05s both; }
.services.is-active .rule       { transform-origin: left center; animation: rule-grow 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.25s both; }

.services.is-active .phones     { animation: phone-in 1.2s cubic-bezier(0.22, 1, 0.36, 1) 0.35s both; }

.services.is-active .kicker-small { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.35s both; }
.services.is-active .title        { animation: title-reveal 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.45s both; }
.services.is-active .item:nth-child(1) { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.65s both; }
.services.is-active .item:nth-child(2) { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.75s both; }
.services.is-active .item:nth-child(3) { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.85s both; }
.services.is-active .item:nth-child(4) { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.95s both; }
.services.is-active .cta-big      { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 1.10s both; }
.services.is-active .bottom       { animation: fade-up 1.0s cubic-bezier(0.22, 1, 0.36, 1) 1.25s both; }
.services.is-active .v-mark       { animation: v-mark-in 1.4s ease-out 0.15s both; }

/* ─── Layout ─── */

.frame {
  position: relative;
  z-index: 1;
  flex: 1;
  padding: 20px 80px 16px;
  display: flex;
  flex-direction: column;
  gap: 22px;
  min-height: 0;
}

.top {
  display: flex;
  align-items: center;
  gap: 16px;
  font-family: system-ui, sans-serif;
}

.page { font-size: 11px; letter-spacing: 0.2em; color: #e87a35; }
.rule { flex: 1; height: 1px; background: rgba(245, 236, 226, 0.4); }
.kicker { font-size: 10px; letter-spacing: 0.25em; text-transform: uppercase; color: rgba(245, 236, 226, 0.9); }

.body {
  flex: 1;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: center;
  min-height: 0;
}

.left {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.phones {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 28px;
}

.phone {
  --w: 240px;
  --h: 500px;
  --pad: 7px;

  width: var(--w);
  height: var(--h);
  padding: var(--pad);
  background: #0c0c0c;
  border-radius: 34px;
  box-shadow:
    0 30px 60px rgba(0, 0, 0, 0.7),
    inset 0 0 0 1px rgba(255, 255, 255, 0.07);
}

.phone--a { transform: translateY(-24px); }
.phone--b { transform: translateY(24px); }

.screen {
  width: 100%;
  height: 100%;
  border-radius: 28px;
  background: #111;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.photo {
  position: relative;
  flex: 0 0 42%;
  padding: 30px 16px 16px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  gap: 4px;
}

.photo--a {
  background:
    linear-gradient(180deg, rgba(0, 0, 0, 0.1) 0%, rgba(17, 17, 17, 0.95) 100%),
    linear-gradient(140deg, #5c2e10, #1a0d05);
}

.photo--b {
  background:
    linear-gradient(180deg, rgba(0, 0, 0, 0.1) 0%, rgba(17, 17, 17, 0.95) 100%),
    linear-gradient(140deg, #8a3a12, #200e04);
}

.notch {
  position: absolute;
  top: 9px;
  left: 50%;
  transform: translateX(-50%);
  width: 42%;
  height: 13px;
  border-radius: 10px;
  background: #000;
}

.photo-meta {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.p-title {
  font-family: Georgia, serif;
  font-size: 22px;
  font-weight: 400;
  letter-spacing: -0.02em;
  color: #fff;
  line-height: 1;
}

.p-sub {
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.6);
  line-height: 1;
}

.p-tag {
  align-self: flex-start;
  margin-top: 8px;
  padding: 4px 10px;
  border-radius: 20px;
  background: #e87a35;
  color: #0c0c0c;
  font-family: system-ui, sans-serif;
  font-size: 9px;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  line-height: 1.2;
}

.panel {
  flex: 1;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  background: #111;
  font-family: system-ui, sans-serif;
  color: #f5ece2;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
  min-height: 0;
}

.hint {
  font-size: 9px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: rgba(245, 236, 226, 0.4);
  line-height: 1;
}

.row { display: flex; gap: 5px; }

.pill {
  flex: 1;
  padding: 10px 0;
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 9px;
  background: rgba(255, 255, 255, 0.05);
  font-family: system-ui, sans-serif;
  font-size: 13px;
  font-weight: 600;
  color: #f5ece2;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2px;
  line-height: 1.1;
  cursor: pointer;
  transition: background 0.15s ease, border-color 0.15s ease;
}

.pill:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(232, 122, 53, 0.6);
}

.pill--on {
  background: rgba(232, 122, 53, 0.15);
  border-color: #e87a35;
  color: #fff;
}

.pill--wide {
  padding: 10px 4px;
  font-size: 10px;
}

.foot {
  margin-top: auto;
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 8px;
}

.price {
  font-family: Georgia, serif;
  font-size: 15px;
  letter-spacing: -0.01em;
  color: #fff;
  line-height: 1;
  white-space: nowrap;
}

.cta {
  padding: 8px 10px;
  border: none;
  border-radius: 20px;
  background: #e87a35;
  color: #0c0c0c;
  font-family: system-ui, sans-serif;
  font-size: 9px;
  font-weight: 700;
  letter-spacing: 0.04em;
  white-space: nowrap;
  line-height: 1.2;
  cursor: pointer;
  transition: background 0.15s ease, transform 0.15s ease;
}

.cta:hover { background: #f0954f; }
.cta:active { transform: scale(0.97); }

.fineprint {
  font-size: 9px;
  line-height: 1.3;
  color: rgba(245, 236, 226, 0.35);
}

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 6px;
}

.cell {
  padding: 11px;
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.04);
  display: flex;
  flex-direction: column;
  gap: 4px;
  font-family: system-ui, sans-serif;
  font-size: 12px;
  color: #f5ece2;
  line-height: 1.15;
  text-align: left;
  cursor: pointer;
  transition: background 0.15s ease, border-color 0.15s ease;
}

.cell:hover {
  background: rgba(255, 255, 255, 0.07);
  border-color: rgba(232, 122, 53, 0.6);
}

.cell--on {
  background: rgba(232, 122, 53, 0.12);
  border-color: #e87a35;
  color: #fff;
}

.cell-n {
  font-size: 8px;
  letter-spacing: 0.18em;
  color: rgba(245, 236, 226, 0.4);
}

.author {
  margin-top: auto;
  padding: 10px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.07);
  display: flex;
  align-items: center;
  gap: 10px;
  min-width: 0;
}

.avatar {
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: #e87a35;
  flex-shrink: 0;
}

.author-meta {
  display: flex;
  flex-direction: column;
  gap: 1px;
  flex: 1;
  min-width: 0;
  overflow: hidden;
}

.author-t {
  font-size: 11px;
  font-weight: 600;
  color: #fff;
  line-height: 1.15;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.author-s {
  font-size: 9px;
  color: rgba(245, 236, 226, 0.5);
  line-height: 1.15;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.right {
  display: flex;
  flex-direction: column;
  gap: 22px;
  max-width: 520px;
}

.kicker-small {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  font-family: system-ui, sans-serif;
  font-size: 11px;
  letter-spacing: 0.28em;
  text-transform: uppercase;
  color: #e87a35;
}

.kicker-small::before {
  content: '';
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #e87a35;
  flex-shrink: 0;
  animation: dot-pulse 2s ease-in-out infinite;
}

.title {
  font-size: clamp(40px, 5vw, 78px);
  font-weight: 400;
  letter-spacing: -0.035em;
  line-height: 0.94;
  color: #f5ece2;
  margin-left: -0.035em;
}

.title em {
  font-style: normal;
  color: #e87a35;
}

.list {
  display: flex;
  flex-direction: column;
  gap: 16px;
  list-style: none;
  margin: 0;
  padding: 0;
}

.item {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.head {
  display: flex;
  align-items: center;
  gap: 10px;
  font-family: system-ui, sans-serif;
}

.dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: #e87a35;
  flex-shrink: 0;
  animation: dot-pulse 2s ease-in-out infinite;
}

.item:nth-child(1) .dot { animation-delay: 0s; }
.item:nth-child(2) .dot { animation-delay: 0.5s; }
.item:nth-child(3) .dot { animation-delay: 1.0s; }
.item:nth-child(4) .dot { animation-delay: 1.5s; }

.line {
  width: 40px;
  height: 1px;
  background: rgba(232, 122, 53, 0.6);
  flex-shrink: 0;
}

.label {
  font-size: 13px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  font-weight: 600;
  color: #f5ece2;
}

.n {
  margin-left: auto;
  font-size: 12px;
  letter-spacing: 0.2em;
  color: rgba(245, 236, 226, 0.4);
}

.text {
  font-family: system-ui, sans-serif;
  font-size: 13px;
  line-height: 1.45;
  color: rgba(245, 236, 226, 0.7);
  padding-left: 16px;
}

.cta-big {
  display: inline-flex;
  align-items: center;
  gap: 18px;
  margin-top: 8px;
  padding: 22px 34px;
  border-radius: 48px;
  background: #e87a35;
  color: #0c0c0c;
  font-family: system-ui, sans-serif;
  font-size: 18px;
  font-weight: 600;
  letter-spacing: 0.02em;
  text-decoration: none;
  width: fit-content;
  cursor: pointer;
  transition: background 0.15s ease, transform 0.15s ease;
}

.cta-big:hover { background: #f0954f; }
.cta-big:active { transform: scale(0.98); }

.arrow {
  font-size: 20px;
  line-height: 1;
}

.bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
  padding-top: 10px;
  border-top: 1px solid rgba(245, 236, 226, 0.18);
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(245, 236, 226, 0.55);
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
  color: rgba(245, 236, 226, 0.4);
  white-space: nowrap;
  pointer-events: none;
}

@media (max-width: 1100px) {
  .body { grid-template-columns: 1fr; gap: 40px; }
  .left { min-height: 540px; }
  .phone { --w: 220px; --h: 460px; }
}

@media (max-width: 700px) {
  .frame { padding: 20px 30px; }
  .phones { gap: 12px; }
  .phone { --w: 160px; --h: 340px; }
  .phone--a, .phone--b { transform: none; }
  .bottom { flex-direction: column; align-items: flex-start; gap: 4px; }
  .v-mark { display: none; }
}

@media (prefers-reduced-motion: reduce) {
  .services:not(.is-active) .top,
  .services:not(.is-active) .rule,
  .services:not(.is-active) .kicker,
  .services:not(.is-active) .phones,
  .services:not(.is-active) .kicker-small,
  .services:not(.is-active) .title,
  .services:not(.is-active) .item,
  .services:not(.is-active) .cta-big,
  .services:not(.is-active) .bottom,
  .services:not(.is-active) .v-mark {
    opacity: 1;
  }
  .services.is-active *,
  .dot,
  .kicker-small::before {
    animation: none !important;
  }
}
</style>