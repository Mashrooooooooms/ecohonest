<script setup>
const steps = [
  { n: '01', title: 'Заявка', text: 'Консультация и оценка задачи.' },
  { n: '02', title: 'Договор', text: 'Фиксируем сроки и стоимость.' },
  { n: '03', title: 'Исполнение', text: 'Собираем данные, готовим документацию.' },
  { n: '04', title: 'Сдача', text: 'Формируем и сдаём отчёты точно в срок.' }
]

const base = import.meta.env.BASE_URL
</script>

<template>
  <section class="about">
    <div class="dim"></div>

    <div class="frame">
      <header class="top">
        <span class="page">02</span>
        <span class="rule"></span>
        <span class="kicker">О нас · Этапы · Объекты</span>
      </header>

      <div class="body">
        <div class="left">
          <span class="kicker-small">— 01 · О компании</span>

          <h2 class="title">
            Честный<br />
            <em>эколог</em>
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
          <span class="kicker-small">— 02 · С объектов</span>

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
          <span class="kicker-small">— 03 · Этапы работ</span>
        </div>
        <ol class="steps">
          <li v-for="s in steps" :key="s.n" class="step">
            <span class="step-n">{{ s.n }}</span>
            <span class="step-t">{{ s.title }}</span>
            <span class="step-d">{{ s.text }}</span>
          </li>
        </ol>
      </footer>
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
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 0.28em;
  text-transform: uppercase;
  color: #c9e265;
}

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
}

.title em {
  font-style: italic;
  color: #c9e265;
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

@media (max-width: 1100px) {
  .body { grid-template-columns: 1fr; gap: 32px; }
  .right { height: auto; }
  .shots { aspect-ratio: 16 / 7; }
  .steps { grid-template-columns: repeat(2, 1fr); gap: 20px; }
}

@media (max-width: 700px) {
  .frame { padding: 20px 30px; }
  .facts { flex-wrap: wrap; gap: 20px; }
  .steps { grid-template-columns: 1fr; }
}
</style>