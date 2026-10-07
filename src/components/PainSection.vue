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

const risks = [
  { code: '8.2 ч.3.4', text: 'Обращение с отходами, повтор', sum: '150–200 тыс. ₽', stop: '—' },
  { code: '8.2 ч.4',   text: 'Размещение отходов без лицензии', sum: 'до 400 тыс. ₽', stop: '—' },
  { code: '8.21 ч.2',  text: 'Выбросы без разрешения', sum: 'до 250 тыс. ₽', stop: '90 сут' },
  { code: '8.13 ч.4',  text: 'Нарушение водного кодекса', sum: 'до 300 тыс. ₽', stop: '90 сут' },
  { code: '8.14',      text: 'Сброс сточных вод, превышение ПДК', sum: '80–100 тыс. ₽', stop: '90 сут' },
  { code: '8.41',      text: 'Просрочка платы за НВОС', sum: '50–100 тыс. ₽', stop: '—' },
  { code: '8.41.1',    text: 'Неуплата экологического сбора', sum: '3× суммы, мин. 500 тыс. ₽', stop: '—' },
  { code: '8.5.1',     text: 'Сокрытие экологической информации', sum: '70–150 тыс. ₽', stop: '—' },
  { code: '8.46',      text: 'Непостановка на учёт объекта НВОС', sum: 'до 100 тыс. ₽', stop: '—' }
]

const cases = [
  { date: '04.03.2025', text: 'ВС РФ № 69-АД25-2-К7 — ч. 4 ст. 8.2 КоАП, размещение отходов', sum: '400 000 ₽' },
  { date: '22.04.2026', text: 'Саратовская обл. — ст. 8.41.1 КоАП, неуплата экосбора', sum: '250 000 ₽' },
  { date: '10.08.2026', text: 'Тисульский райсуд, Кузбасс — 3 золотодобывающих предприятия', sum: '90 суток' }
]

const deadlines = [
  { d: '02.02', t: '2-ТП (отходы)' },
  { d: '01.03', t: 'Плата за НВОС' },
  { d: '10.03', t: 'Декларация о плате' },
  { d: '15.04', t: 'Экологический сбор' }
]

const tickerItems = [
  'КоАП 8.2 — до 400 000 ₽',
  'КоАП 8.21 — до 250 000 ₽ · приостановка до 90 суток',
  'КоАП 8.13 — до 300 000 ₽',
  'КоАП 8.41.1 — 3× суммы, не менее 500 000 ₽',
  'КоАП 8.5.1 — до 150 000 ₽ за сокрытие данных'
]
</script>

<template>
  <section ref="root" class="pain" :class="{ 'pain--in': isActive || entered }">
    <div class="vignette"></div>

    <div class="ticker ticker--top">
      <div class="ticker-track">
        <span v-for="(t, i) in tickerItems" :key="`t-${i}`" class="ticker-item">{{ t }}</span>
        <span v-for="(t, i) in tickerItems" :key="`t-d-${i}`" class="ticker-item">{{ t }}</span>
      </div>
    </div>

    <div class="frame">
      <header class="top">
        <span class="page">03</span>
        <span class="rule"></span>
        <span class="kicker">Риски · КоАП · Сроки</span>
      </header>

      <div class="left">
        <h2>
          Цена<br />
          <em>ошибки</em><br />
          &nbsp;
        </h2>

        <p class="lead">
          Один неверный показатель в отчёте — статья, штраф, приостановка.
          Ниже — санкции главы 8 КоАП РФ для юридических лиц на 2026 год.
        </p>

        <div class="meta">
          <div class="meta-item">
            <span class="meta-n">9</span>
            <span class="meta-t">составов</span>
          </div>
          <div class="meta-item">
            <span class="meta-n">1 млн</span>
            <span class="meta-t">макс. штраф</span>
          </div>
          <div class="meta-item">
            <span class="meta-n">90</span>
            <span class="meta-t">суток стоп</span>
          </div>
        </div>
      </div>

      <div class="sliders">
        <div class="slide slide--table">
          <div class="table">
            <div class="thead">
              <span>КоАП</span>
              <span>Нарушение</span>
              <span class="ta-r">Штраф юрлицу</span>
              <span class="ta-r">Стоп</span>
            </div>

            <div v-for="r in risks" :key="r.code" class="tr">
              <span class="td-code">{{ r.code }}</span>
              <span class="td-text">{{ r.text }}</span>
              <span class="td-sum">{{ r.sum }}</span>
              <span class="td-stop" :class="{ 'td-stop--yes': r.stop !== '—' }">{{ r.stop }}</span>
            </div>
          </div>
        </div>

        <div class="slide slide--cases">
          <div class="block block--cases">
            <span class="block-head">Судебная практика</span>
            <ul class="cases">
              <li v-for="c in cases" :key="c.date" class="case">
                <span class="case-date">{{ c.date }}</span>
                <span class="case-text">{{ c.text }}</span>
                <span class="case-sum">{{ c.sum }}</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="slide slide--dates">
          <div class="block block--dates">
            <span class="block-head">Сроки отчётности</span>
            <ul class="dates">
              <li v-for="d in deadlines" :key="d.d" class="date">
                <span class="date-d">{{ d.d }}</span>
                <span class="date-t">{{ d.t }}</span>
              </li>
            </ul>
            <p class="fine">Декларация НВОС — п. 8 ст. 16.4 ФЗ № 7-ФЗ. Экосбор — ст. 24.5 ФЗ № 89-ФЗ. Просрочка = ст. 8.41 / 8.41.1 КоАП.</p>
          </div>
        </div>
      </div>

      <span class="v-mark">Ecohonest · 2026</span>
    </div>

    <div class="ticker ticker--bottom">
      <div class="ticker-track ticker-track--reverse">
        <span v-for="(t, i) in tickerItems" :key="`b-${i}`" class="ticker-item">{{ t }}</span>
        <span v-for="(t, i) in tickerItems" :key="`b-d-${i}`" class="ticker-item">{{ t }}</span>
      </div>
    </div>
  </section>
</template>

<style scoped>
.pain {
  position: relative;
  height: 100vh;
  height: 100dvh;
  padding: 50px 0;
  display: flex;
  flex-direction: column;
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.15) 0%,
    rgba(60, 15, 5, 0.3) 50%,
    rgba(0, 0, 0, 0.65) 100%
  );
  color: #f5ece2;
  font-family: Georgia, "Times New Roman", serif;
  overflow: hidden;
  box-sizing: border-box;
}

.vignette {
  position: absolute;
  inset: 0;
  background: radial-gradient(
    ellipse 85% 100% at 55% 50%,
    rgba(35, 8, 2, 0.78) 0%,
    rgba(35, 8, 2, 0.42) 60%,
    rgba(35, 8, 2, 0.05) 100%
  );
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

@keyframes ticker-in {
  from { opacity: 0; }
  to   { opacity: 1; }
}

.pain:not(.pain--in) .top,
.pain:not(.pain--in) .rule,
.pain:not(.pain--in) .kicker,
.pain:not(.pain--in) h2,
.pain:not(.pain--in) .lead,
.pain:not(.pain--in) .meta,
.pain:not(.pain--in) .thead,
.pain:not(.pain--in) .tr,
.pain:not(.pain--in) .block,
.pain:not(.pain--in) .v-mark,
.pain:not(.pain--in) .ticker {
  opacity: 0;
}

.pain.pain--in .ticker        { animation: ticker-in 1.2s ease-out both; }
.pain.pain--in .top           { animation: fade-right 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.05s both; }
.pain.pain--in .rule          { transform-origin: left center; animation: rule-grow 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.25s both; }

.pain.pain--in h2             { animation: title-reveal 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.35s both; }
.pain.pain--in .lead          { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.60s both; }
.pain.pain--in .meta          { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.78s both; }

.pain.pain--in .thead         { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.42s both; }
.pain.pain--in .tr:nth-child(2)  { animation: fade-up 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.52s both; }
.pain.pain--in .tr:nth-child(3)  { animation: fade-up 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.57s both; }
.pain.pain--in .tr:nth-child(4)  { animation: fade-up 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.62s both; }
.pain.pain--in .tr:nth-child(5)  { animation: fade-up 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.67s both; }
.pain.pain--in .tr:nth-child(6)  { animation: fade-up 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.72s both; }
.pain.pain--in .tr:nth-child(7)  { animation: fade-up 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.77s both; }
.pain.pain--in .tr:nth-child(8)  { animation: fade-up 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.82s both; }
.pain.pain--in .tr:nth-child(9)  { animation: fade-up 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.87s both; }
.pain.pain--in .tr:nth-child(10) { animation: fade-up 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.92s both; }

.pain.pain--in .block--cases  { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 1.00s both; }
.pain.pain--in .block--dates  { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 1.15s both; }

.pain.pain--in .v-mark        { animation: v-mark-in 1.4s ease-out 0.15s both; }

/* ═════════ Layout ═════════ */

.ticker {
  position: relative;
  z-index: 1;
  flex-shrink: 0;
  border-top: 1px solid rgba(245, 236, 226, 0.25);
  border-bottom: 1px solid rgba(245, 236, 226, 0.25);
  padding: 8px 0;
  overflow: hidden;
}

.ticker-track {
  display: inline-flex;
  align-items: center;
  gap: 40px;
  white-space: nowrap;
  animation: ticker 50s linear infinite;
  font-family: system-ui, sans-serif;
  font-size: 11px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: rgba(245, 236, 226, 0.7);
}

.ticker-track--reverse { animation-direction: reverse; }

.ticker-item::before {
  content: '·';
  margin-right: 40px;
  color: #e87a35;
}

@keyframes ticker {
  from { transform: translateX(0); }
  to { transform: translateX(-50%); }
}

.frame {
  position: relative;
  z-index: 1;
  flex: 1;
  padding: 14px 70px 12px;
  display: grid;
  grid-template-columns: 0.75fr 1.65fr;
  grid-template-rows: auto 1fr auto;
  gap: 12px 46px;
  min-height: 0;
}

.top {
  grid-column: 1 / -1;
  grid-row: 1;
  display: flex;
  align-items: center;
  gap: 16px;
  font-family: system-ui, sans-serif;
}

.page { font-size: 11px; letter-spacing: 0.2em; color: #e87a35; }
.rule { flex: 1; height: 1px; background: rgba(245, 236, 226, 0.4); }
.kicker { font-size: 10px; letter-spacing: 0.25em; text-transform: uppercase; color: rgba(245, 236, 226, 0.9); }

.left {
  grid-column: 1;
  grid-row: 2;
  align-self: center;
  display: flex;
  flex-direction: column;
  gap: 14px;
  min-width: 0;
}

h2 {
  font-size: clamp(42px, 5vw, 76px);
  font-weight: 400;
  letter-spacing: -0.045em;
  line-height: 0.9;
  color: #f5ece2;
  margin-left: -0.045em;
}

h2 em {
  font-style: normal;
  color: #e87a35;
}

.lead {
  font-family: system-ui, sans-serif;
  font-size: 13px;
  line-height: 1.5;
  color: rgba(245, 236, 226, 0.72);
  max-width: 340px;
}

.meta {
  display: flex;
  gap: 24px;
  padding-top: 10px;
  border-top: 1px solid rgba(245, 236, 226, 0.15);
}

.meta-item { display: flex; flex-direction: column; gap: 2px; }

.meta-n {
  font-family: Georgia, serif;
  font-size: 22px;
  line-height: 1;
  color: #e87a35;
}

.meta-t {
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: rgba(245, 236, 226, 0.55);
}

/* ─── Sliders: на десктопе прозрачная обёртка ─── */
.sliders { display: contents; }

.slide { min-width: 0; }

.slide--table {
  grid-column: 2;
  grid-row: 2;
  align-self: center;
}

.slide--cases {
  grid-column: 1;
  grid-row: 3;
}

.slide--dates {
  grid-column: 2;
  grid-row: 3;
}

/* ─── Таблица ─── */
.table {
  width: 100%;
  display: flex;
  flex-direction: column;
  font-family: system-ui, sans-serif;
  border-top: 1px solid rgba(245, 236, 226, 0.35);
}

.thead,
.tr {
  display: grid;
  grid-template-columns: 88px minmax(0, 1fr) 170px 72px;
  column-gap: 20px;
  align-items: center;
  padding: 7px 0;
  border-bottom: 1px solid rgba(245, 236, 226, 0.1);
}

.thead {
  font-size: 10px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: rgba(245, 236, 226, 0.45);
  border-bottom-color: rgba(245, 236, 226, 0.28);
}

.ta-r { text-align: right; }

.td-code {
  font-size: 12px;
  letter-spacing: 0.04em;
  color: #e87a35;
  white-space: nowrap;
  font-weight: 600;
}

.td-text {
  font-size: 13px;
  line-height: 1.3;
  color: rgba(245, 236, 226, 0.85);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.td-sum {
  font-size: 13px;
  font-weight: 600;
  color: #f5ece2;
  text-align: right;
  white-space: nowrap;
}

.td-stop {
  font-size: 11px;
  text-align: right;
  color: rgba(245, 236, 226, 0.35);
  white-space: nowrap;
}

.td-stop--yes {
  color: #d14028;
  font-weight: 700;
}

/* ─── Практика и сроки ─── */
.block {
  display: flex;
  flex-direction: column;
  gap: 8px;
  min-width: 0;
}

.block-head {
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 0.24em;
  text-transform: uppercase;
  color: #e87a35;
}

.cases { display: flex; flex-direction: column; gap: 6px; list-style: none; margin: 0; padding: 0; }

.case {
  display: grid;
  grid-template-columns: 78px minmax(0, 1fr) 92px;
  gap: 14px;
  align-items: center;
  font-family: system-ui, sans-serif;
}

.case-date {
  font-size: 11px;
  letter-spacing: 0.06em;
  color: rgba(245, 236, 226, 0.5);
}

.case-text {
  font-size: 12px;
  line-height: 1.35;
  color: rgba(245, 236, 226, 0.82);
}

.case-sum {
  font-size: 13px;
  font-weight: 700;
  color: #f5ece2;
  text-align: right;
  white-space: nowrap;
}

.dates { display: flex; gap: 22px; flex-wrap: wrap; list-style: none; margin: 0; padding: 0; }

.date { display: flex; flex-direction: column; gap: 2px; font-family: system-ui, sans-serif; }

.date-d {
  font-family: Georgia, serif;
  font-size: 20px;
  line-height: 1;
  color: #e87a35;
}

.date-t {
  font-size: 10px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: rgba(245, 236, 226, 0.6);
}

.fine {
  font-family: system-ui, sans-serif;
  font-size: 10px;
  line-height: 1.4;
  color: rgba(245, 236, 226, 0.45);
  margin-top: 4px;
}

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

/* ─── Средние экраны ─── */
@media (max-width: 1200px) and (min-width: 769px) {
  .frame {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto auto auto auto;
    gap: 14px;
    padding: 14px 40px;
    overflow-y: auto;
    scrollbar-width: none;
  }
  .frame::-webkit-scrollbar { display: none; }

  .top { grid-column: 1; grid-row: 1; }
  .left { grid-column: 1; grid-row: 2; align-self: auto; }

  .sliders { display: contents; }
  .slide--table { grid-column: 1; grid-row: 3; align-self: auto; }
  .slide--cases { grid-column: 1; grid-row: 4; }
  .slide--dates { grid-column: 1; grid-row: 5; }
}

/* ═════════ Мобила (портрет) — таблица + сроки, без практики ═════════ */

@media (max-width: 768px) and (orientation: portrait) {
  .pain {
    height: 100dvh;
    padding: 0;
    overflow: hidden;
    background: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.4) 0%,
      rgba(60, 15, 5, 0.4) 45%,
      rgba(0, 0, 0, 0.78) 100%
    );
  }

  .frame {
    display: flex;
    flex-direction: column;
    gap: 24px;
    /* сдвигаем весь контент вниз */
    padding: 28px 20px 24px;
    overflow-y: auto;
    overflow-x: hidden;
    scrollbar-width: none;
  }
  .frame::-webkit-scrollbar { display: none; }

  .top { gap: 10px; }
  .page { font-size: 10px; letter-spacing: 0.18em; }
  .kicker { font-size: 9px; letter-spacing: 0.18em; }

  .left {
    gap: 10px;
    align-self: auto;
    /* лёгкий отступ сверху, чтобы заголовок не лип к шапке */
    padding-top: 6px;
  }

  h2 {
    font-size: clamp(34px, 11vw, 48px);
    line-height: 0.9;
  }

  .lead { display: none; }

  .meta {
    gap: 20px;
    padding-top: 8px;
  }

  .meta-n { font-size: 20px; }
  .meta-t { font-size: 9px; letter-spacing: 0.1em; }

  /* слайдеры → обычный вертикальный поток */
  .sliders {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .slide { flex: none; min-width: 0; }

  .slide--cases { display: none; }

  .slide--table,
  .slide--dates {
    grid-column: auto;
    grid-row: auto;
    align-self: auto;
  }

  .thead,
  .tr {
    grid-template-columns: 68px minmax(0, 1fr) 110px;
    column-gap: 10px;
    padding: 6px 0;
  }

  .thead span:last-child,
  .td-stop { display: none; }

  .thead { font-size: 9px; letter-spacing: 0.14em; }

  .td-code { font-size: 11px; }
  .td-text {
    font-size: 11.5px;
    line-height: 1.25;
    white-space: normal;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    overflow-wrap: anywhere;
    word-break: break-word;
  }
  .td-sum { font-size: 11.5px; }

  .block-head { font-size: 9px; letter-spacing: 0.2em; }
  .block--dates { gap: 6px; }

  .dates {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 8px 14px;
  }

  .date { gap: 1px; }
  .date-d { font-size: 16px; }
  .date-t { font-size: 8.5px; letter-spacing: 0.08em; line-height: 1.2; }

  .fine { display: none; }

  .ticker { padding: 6px 0; }
  .ticker-track {
    gap: 28px;
    font-size: 9.5px;
    letter-spacing: 0.18em;
    animation-duration: 38s;
  }
  .ticker-item::before { margin-right: 28px; }

  .v-mark { display: none; }
}

@media (max-width: 380px) and (orientation: portrait) {
  h2 { font-size: 34px; }
  .meta-n { font-size: 18px; }

  .thead,
  .tr {
    grid-template-columns: 62px minmax(0, 1fr) 96px;
    column-gap: 8px;
  }
  .td-text { font-size: 11px; }
  .td-sum { font-size: 11px; }

  .date-d { font-size: 14px; }
  .date-t { font-size: 8px; letter-spacing: 0.06em; }
}

/* ═════════ Мобила (ландшафт) ═════════ */

@media (orientation: landscape) and (max-height: 500px) {
  .pain {
    height: 100dvh;
    padding: 0;
    overflow: hidden;
  }

  .frame {
    padding: 12px 24px 14px;
    gap: 10px 28px;
    overflow-y: auto;
    scrollbar-width: none;
  }
  .frame::-webkit-scrollbar { display: none; }

  .sliders { display: contents; }

  .lead { display: none; }

  h2 { font-size: 40px; }

  .meta { gap: 18px; padding-top: 8px; }
  .meta-n { font-size: 17px; }
  .meta-t { font-size: 9px; }

  .thead,
  .tr {
    grid-template-columns: 70px minmax(0, 1fr) 110px 54px;
    column-gap: 12px;
    padding: 4px 0;
  }

  .thead { font-size: 9px; }
  .td-code { font-size: 11px; }
  .td-text {
    font-size: 11.5px;
    white-space: nowrap;
    display: block;
    -webkit-line-clamp: unset;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .td-sum { font-size: 11.5px; }
  .td-stop { font-size: 10px; }

  .block { gap: 6px; }
  .block-head { font-size: 9px; letter-spacing: 0.2em; }

  .cases { gap: 4px; }
  .case { grid-template-columns: 70px minmax(0, 1fr) 84px; gap: 10px; }
  .case-date { font-size: 10px; }
  .case-text {
    font-size: 11px;
    display: block;
    -webkit-line-clamp: unset;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .case-sum { font-size: 12px; }

  .dates { display: flex; flex-wrap: wrap; gap: 18px; }
  .date-d { font-size: 16px; }
  .date-t { font-size: 8.5px; }

  .fine { display: none; }
  .v-mark { display: none; }
}

/* ═════════ Reduced motion ═════════ */

@media (prefers-reduced-motion: reduce) {
  .pain:not(.pain--in) .top,
  .pain:not(.pain--in) .rule,
  .pain:not(.pain--in) .kicker,
  .pain:not(.pain--in) h2,
  .pain:not(.pain--in) .lead,
  .pain:not(.pain--in) .meta,
  .pain:not(.pain--in) .thead,
  .pain:not(.pain--in) .tr,
  .pain:not(.pain--in) .block,
  .pain:not(.pain--in) .v-mark,
  .pain:not(.pain--in) .ticker {
    opacity: 1;
  }
  .pain.pain--in *,
  .ticker-track {
    animation: none !important;
  }
}
</style>