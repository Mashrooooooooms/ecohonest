<script setup>
import { ref, onMounted } from 'vue'

const name = ref('')
const phone = ref('')
const topic = ref('')
const sent = ref(false)
const ready = ref(false)

onMounted(() => {
  requestAnimationFrame(() => requestAnimationFrame(() => (ready.value = true)))
})

function submit() {
  sent.value = true
}
</script>

<template>
  <section class="page" :class="{ ready }">
    <div class="photo"></div>
    <div class="shade"></div>

    <header class="masthead fade" style="--d:.05s">
      <span class="brand">Ecohonest</span>
      <span class="dot">◦</span>
      <span class="mid">- 05 - / Form</span>
      <span class="dot">◦</span>
      <span class="right">Moscow — MMXXVI</span>
    </header>

    <main class="body">
      <div class="left">
        <span class="ghost" aria-hidden="true">05</span>

        <div class="stack">
          <p class="kicker fade" style="--d:.15s">
            <span class="rule"></span>
            <span>Заявка / Consultation</span>
          </p>

          <h1 class="title">
            <span class="mask">
              <span class="word rise" style="--d:.28s">Оставьте</span>
            </span>
            <span class="mask">
              <span class="word rise word--accent" style="--d:.44s">заявку</span>
            </span>
          </h1>

          <p class="lead fade" style="--d:.62s">
            Первая консультация бесплатна. Ответим
            в&nbsp;течение рабочего дня и&nbsp;предложим сроки.
          </p>
        </div>

        <ul class="meta fade" style="--d:.8s">
          <li>
            <span class="m-n">I</span>
            <span class="m-k">Телефон</span>
            <a href="tel:+79915917778">+7 991 591 77 78</a>
          </li>
          <li>
            <span class="m-n">II</span>
            <span class="m-k">Почта</span>
            <a href="mailto:info@ecohonest.ru">info@ecohonest.ru</a>
          </li>
          <li>
            <span class="m-n">III</span>
            <span class="m-k">Адрес</span>
            <span>Москва, ул. М. Поливановой 9</span>
          </li>
        </ul>
      </div>

      <div class="right">
        <div class="divider fade" style="--d:.4s"></div>

        <form v-if="!sent" class="form" @submit.prevent="submit">
          <div class="form-top fade" style="--d:.3s">
            <span>Заполните форму</span>
            <span class="cnt">01 / 01</span>
          </div>

          <label class="field fade" style="--d:.4s">
            <span class="f-num">01</span>
            <span class="f-label">Имя</span>
            <input v-model="name" type="text" placeholder="—" required>
          </label>

          <label class="field fade" style="--d:.5s">
            <span class="f-num">02</span>
            <span class="f-label">Телефон</span>
            <input v-model="phone" type="tel" placeholder="—" required>
          </label>

          <label class="field field--area fade" style="--d:.6s">
            <span class="f-num">03</span>
            <span class="f-label">Задача</span>
            <textarea v-model="topic" rows="2" placeholder="—"></textarea>
          </label>

          <button class="send fade" style="--d:.72s" type="submit">
            <span class="send-bg"></span>
            <span class="send-text">Отправить заявку</span>
            <span class="send-arrow">→</span>
          </button>

          <p class="note fade" style="--d:.82s">
            Отправка = согласие с&nbsp;обработкой персональных данных.
          </p>
        </form>

        <div v-else class="done">
          <span class="done-mark rise" style="--d:.05s">✓</span>
          <p class="done-title rise" style="--d:.15s">
            Заявка<br>принята
          </p>
          <p class="done-sub fade" style="--d:.4s">
            Свяжемся с&nbsp;вами в&nbsp;течение рабочего дня.
          </p>
          <button
            class="send send--alt fade"
            style="--d:.5s"
            type="button"
            @click="sent = false"
          >
            <span class="send-bg"></span>
            <span class="send-text">Новая заявка</span>
            <span class="send-arrow">→</span>
          </button>
        </div>
      </div>
    </main>

    <footer class="colophon fade" style="--d:.9s">
      <span>Ecohonest Studio</span>
      <span>Ecological&nbsp;Audit — 2026</span>
      <span>+7 991 591 77 78</span>
    </footer>
  </section>
</template>

<style scoped>
.page {
  --bg:      #070e1a;
  --fg:      #f0f5fc;
  --muted:   rgba(240, 245, 252, 0.7);
  --faint:   rgba(240, 245, 252, 0.38);
  --hair:    rgba(240, 245, 252, 0.16);
  --accent:  #6a97ff;
  --accent-d: rgba(106, 151, 255, 0.35);

  position: relative;
  height: 100vh;
  display: grid;
  grid-template-rows: 64px 1fr 60px;
  color: var(--fg);
  background: var(--bg);
  font-family: Georgia, "Times New Roman", serif;
  overflow: hidden;
  isolation: isolate;
}

/* ─── фото ─── */
.photo {
  position: absolute;
  inset: 0;
  z-index: -2;
  background: url('/bg3.jpg') center / cover no-repeat;
  filter: saturate(1.08) contrast(1.04) brightness(0.72);
  transform: scale(1.06);
  animation: photoIn 20s ease-out both;
}

/* затемнение: слева плотно под текст, справа глубже под форму,
   но фото остаётся видимым по краям и в световых пятнах */
.shade {
  position: absolute;
  inset: 0;
  z-index: -1;
  pointer-events: none;
  background:
    linear-gradient(
      90deg,
      rgba(7, 14, 26, 0.62) 0%,
      rgba(7, 14, 26, 0.55) 30%,
      rgba(7, 14, 26, 0.5) 45%,
      rgba(7, 14, 26, 0.6) 61.5%,
      rgba(7, 14, 26, 0.74) 100%
    ),
    /* мягкое затемнение сверху и снизу — фокус в центре */
    radial-gradient(
      130% 95% at 50% 50%,
      rgba(0, 0, 0, 0) 40%,
      rgba(0, 0, 0, 0.55) 100%
    );
}

@keyframes photoIn {
  from { transform: scale(1.14) translate3d(10px, 0, 0); }
  to   { transform: scale(1.02) translate3d(0, 0, 0); }
}

/* ─── верхняя линейка ─── */
.masthead {
  position: relative;
  z-index: 2;
  display: flex;
  align-items: center;
  gap: 18px;
  padding: 0 40px;
  font-family: system-ui, -apple-system, "Inter", sans-serif;
  font-size: 10px;
  letter-spacing: 0.34em;
  text-transform: uppercase;
  color: var(--muted);
  border-bottom: 1px solid var(--hair);
  text-shadow: 0 1px 12px rgba(0, 0, 0, 0.85);
}
.brand { color: var(--fg); font-weight: 500; }
.dot   { color: var(--accent-d); font-size: 14px; line-height: 1; }
.mid   { color: var(--accent); }
.right { margin-left: auto; }

/* ─── разворот 7/5 ─── */
.body {
  position: relative;
  z-index: 2;
  display: grid;
  grid-template-columns: minmax(0, 7fr) minmax(0, 5fr);
  min-height: 0;
}

/* ─── левая страница ─── */
.left {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 56px 64px 48px 88px;
  min-width: 0;
}

/* локальный скрим под текстовым блоком — гарантирует контраст
   поверх любых световых пятен фотографии */
.left::before {
  content: "";
  position: absolute;
  inset: 0;
  pointer-events: none;
  background:
    radial-gradient(
      70% 60% at 32% 38%,
      rgba(7, 14, 26, 0.55) 0%,
      rgba(7, 14, 26, 0.3) 45%,
      rgba(7, 14, 26, 0) 78%
    ),
    radial-gradient(
      60% 40% at 30% 88%,
      rgba(7, 14, 26, 0.5) 0%,
      rgba(7, 14, 26, 0) 70%
    );
}

.ghost {
  position: absolute;
  left: -20px;
  bottom: -110px;
  font-family: Georgia, serif;
  font-size: clamp(320px, 34vw, 520px);
  font-weight: 400;
  line-height: 0.78;
  letter-spacing: -0.08em;
  color: var(--accent);
  opacity: 0.06;
  pointer-events: none;
  user-select: none;
  animation: ghostDrift 24s ease-in-out infinite alternate;
}
@keyframes ghostDrift {
  from { transform: translate3d(0, 0, 0); }
  to   { transform: translate3d(24px, -16px, 0); }
}

.stack {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 30px;
  max-width: 620px;
}

.kicker {
  display: flex;
  align-items: center;
  gap: 16px;
  margin: 0;
  font-family: system-ui, -apple-system, "Inter", sans-serif;
  font-size: 10px;
  letter-spacing: 0.44em;
  text-transform: uppercase;
  color: var(--muted);
  text-shadow: 0 1px 16px rgba(0, 0, 0, 0.9);
}
.kicker .rule {
  display: block;
  width: 60px;
  height: 1px;
  background: var(--accent);
  transform-origin: left center;
  box-shadow: 0 0 12px rgba(106, 151, 255, 0.55);
  animation: ruleGrow 1.2s cubic-bezier(.2, .9, .25, 1) .45s both;
}
@keyframes ruleGrow {
  from { transform: scaleX(0); }
  to   { transform: scaleX(1); }
}

/* заголовок — усиленные тени, чтобы держал контраст */
.title {
  margin: 0;
  font-size: clamp(72px, 8.6vw, 168px);
  font-weight: 400;
  line-height: 0.84;
  letter-spacing: -0.055em;
  color: var(--fg);
  text-shadow:
    0 1px 2px rgba(0, 0, 0, 0.9),
    0 4px 24px rgba(0, 0, 0, 0.85),
    0 12px 90px rgba(0, 0, 0, 0.8);
}
.mask {
  display: block;
  overflow: hidden;
  padding: 0.04em 0.1em 0.14em 0;
}
.word {
  display: block;
  margin-left: 0.02em;
}
.word--accent {
  color: var(--accent);
  font-style: italic;
  text-shadow:
    0 1px 2px rgba(0, 0, 0, 0.9),
    0 4px 24px rgba(0, 0, 0, 0.8),
    0 12px 70px rgba(106, 151, 255, 0.55);
}

.lead {
  max-width: 380px;
  margin: 0;
  font-family: Georgia, serif;
  font-size: 15.5px;
  line-height: 1.6;
  color: rgba(240, 245, 252, 0.96);
  text-shadow:
    0 1px 2px rgba(0, 0, 0, 0.9),
    0 2px 18px rgba(0, 0, 0, 0.9);
}

/* контакты */
.meta {
  position: relative;
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  max-width: 560px;
  border-top: 1px solid var(--hair);
}
.meta li {
  display: grid;
  grid-template-columns: 28px 96px 1fr;
  align-items: baseline;
  gap: 16px;
  padding: 12px 0;
  border-bottom: 1px solid var(--hair);
  font-family: system-ui, -apple-system, "Inter", sans-serif;
  font-size: 11px;
  letter-spacing: 0.08em;
}
.m-n {
  font-family: Georgia, serif;
  font-size: 12px;
  letter-spacing: 0.1em;
  color: var(--accent);
  text-shadow: 0 0 14px rgba(106, 151, 255, 0.6);
}
.m-k {
  font-size: 10px;
  letter-spacing: 0.24em;
  text-transform: uppercase;
  color: var(--muted);
}
.meta a,
.meta span:last-child {
  color: var(--fg);
  text-decoration: none;
  text-shadow:
    0 1px 2px rgba(0, 0, 0, 0.9),
    0 2px 16px rgba(0, 0, 0, 0.9);
  position: relative;
  transition: color 0.3s ease;
}
.meta a::after {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  bottom: -3px;
  height: 1px;
  background: var(--accent);
  transform: scaleX(0);
  transform-origin: left center;
  transition: transform 0.45s cubic-bezier(.2, .9, .25, 1);
}
.meta a:hover { color: var(--accent); }
.meta a:hover::after { transform: scaleX(1); }

/* ─── правая страница ─── */
.right {
  position: relative;
  display: flex;
  align-items: center;
  padding: 48px 88px 48px 56px;
  min-width: 0;
}

.divider {
  position: absolute;
  left: 0;
  top: 8%;
  bottom: 8%;
  width: 1px;
  background: linear-gradient(
    180deg,
    transparent 0%,
    var(--accent) 30%,
    var(--accent) 70%,
    transparent 100%
  );
  opacity: 0.75;
  transform-origin: center top;
}
.divider.anim { transform: scaleY(0); }
.ready .divider { animation: lineDrop 1.2s cubic-bezier(.2, .9, .25, 1) both; }
@keyframes lineDrop {
  from { transform: scaleY(0); }
  to   { transform: scaleY(1); }
}

.form,
.done {
  width: 100%;
  max-width: 460px;
  display: flex;
  flex-direction: column;
}

.form-top {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  padding-bottom: 20px;
  margin-bottom: 6px;
  border-bottom: 1px solid var(--hair);
  font-family: system-ui, -apple-system, "Inter", sans-serif;
  font-size: 10px;
  letter-spacing: 0.34em;
  text-transform: uppercase;
  color: var(--muted);
  text-shadow: 0 1px 14px rgba(0, 0, 0, 0.9);
}
.cnt { color: var(--accent); }

.field {
  display: grid;
  grid-template-columns: 28px 92px 1fr;
  align-items: baseline;
  gap: 16px;
  padding: 16px 0;
  border-bottom: 1px solid var(--hair);
  font-family: system-ui, -apple-system, "Inter", sans-serif;
  transition: border-color 0.35s ease;
}
.field--area { align-items: start; padding-top: 18px; }

.f-num {
  font-family: Georgia, serif;
  font-size: 13px;
  letter-spacing: 0.08em;
  color: var(--accent);
  line-height: 1.2;
  text-shadow: 0 0 14px rgba(106, 151, 255, 0.55);
}
.f-label {
  font-size: 10px;
  letter-spacing: 0.26em;
  text-transform: uppercase;
  color: var(--muted);
  line-height: 1.4;
  padding-top: 2px;
  transition: color 0.3s ease;
}

.field input,
.field textarea {
  width: 100%;
  min-width: 0;
  padding: 0;
  background: transparent;
  border: none;
  outline: none;
  resize: none;
  color: var(--fg);
  font-family: Georgia, serif;
  font-size: 18px;
  letter-spacing: -0.01em;
  line-height: 1.3;
  caret-color: var(--accent);
  text-shadow:
    0 1px 2px rgba(0, 0, 0, 0.9),
    0 2px 16px rgba(0, 0, 0, 0.85);
}
.field textarea { font-size: 15px; line-height: 1.45; }

.field input::placeholder,
.field textarea::placeholder { color: var(--faint); }

.field:focus-within { border-bottom-color: var(--accent); }
.field:focus-within .f-label { color: var(--accent); }

.send {
  position: relative;
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  gap: 20px;
  margin-top: 28px;
  padding: 20px 24px;
  background: transparent;
  border: 1px solid rgba(240, 245, 252, 0.36);
  color: var(--fg);
  font-family: system-ui, -apple-system, "Inter", sans-serif;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.36em;
  text-transform: uppercase;
  cursor: pointer;
  overflow: hidden;
  isolation: isolate;
  text-shadow: 0 1px 14px rgba(0, 0, 0, 0.9);
  transition: color 0.4s ease, border-color 0.4s ease;
}

.send-bg {
  position: absolute;
  inset: 0;
  z-index: -1;
  background: linear-gradient(90deg, var(--accent), #8bb0ff);
  transform: translateX(-101%);
  transition: transform 0.6s cubic-bezier(.2, .9, .25, 1);
}
.send-text,
.send-arrow {
  position: relative;
  transition: transform 0.5s cubic-bezier(.2, .9, .25, 1);
}
.send:hover {
  color: #061224;
  border-color: var(--accent);
  text-shadow: none;
}
.send:hover .send-bg { transform: translateX(0); }
.send:hover .send-arrow { transform: translateX(10px); }

.send-arrow { font-size: 16px; line-height: 1; }
.send--alt { margin-top: 20px; }

.note {
  margin: 18px 0 0;
  font-family: system-ui, -apple-system, "Inter", sans-serif;
  font-size: 10px;
  letter-spacing: 0.06em;
  line-height: 1.6;
  color: var(--muted);
  text-shadow: 0 1px 12px rgba(0, 0, 0, 0.9);
}

.done { gap: 8px; }

.done-mark {
  font-family: Georgia, serif;
  font-size: 40px;
  line-height: 1;
  color: var(--accent);
  text-shadow:
    0 2px 24px rgba(0, 0, 0, 0.85),
    0 0 50px rgba(106, 151, 255, 0.75);
}
.done-title {
  margin: 6px 0 12px;
  font-family: Georgia, serif;
  font-size: clamp(48px, 4.6vw, 64px);
  line-height: 0.94;
  letter-spacing: -0.045em;
  color: var(--fg);
  text-shadow:
    0 1px 2px rgba(0, 0, 0, 0.9),
    0 4px 24px rgba(0, 0, 0, 0.85),
    0 12px 70px rgba(0, 0, 0, 0.8);
}
.done-sub {
  margin: 0 0 12px;
  font-family: Georgia, serif;
  font-size: 15px;
  line-height: 1.55;
  color: rgba(240, 245, 252, 0.92);
  max-width: 320px;
  text-shadow: 0 2px 16px rgba(0, 0, 0, 0.9);
}

/* ─── нижняя линейка ─── */
.colophon {
  position: relative;
  z-index: 2;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  align-items: center;
  padding: 0 40px;
  border-top: 1px solid var(--hair);
  font-family: system-ui, -apple-system, "Inter", sans-serif;
  font-size: 10px;
  letter-spacing: 0.34em;
  text-transform: uppercase;
  color: var(--muted);
  text-shadow: 0 1px 14px rgba(0, 0, 0, 0.9);
}
.colophon span:nth-child(2) { text-align: center; }
.colophon span:nth-child(3) {
  text-align: right;
  color: rgba(240, 245, 252, 0.92);
}

/* ─── анимации ─── */
.fade { opacity: 0; }
.ready .fade {
  animation: fadeUp 1s cubic-bezier(.2, .9, .25, 1) both;
  animation-delay: var(--d, 0s);
}
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(16px); }
  to   { opacity: 1; transform: translateY(0); }
}

.word.rise { transform: translateY(110%); }
.ready .word.rise {
  animation: wordRise 1.15s cubic-bezier(.2, .9, .25, 1) both;
  animation-delay: var(--d, .3s);
}
@keyframes wordRise {
  from { transform: translateY(110%); }
  to   { transform: translateY(0); }
}

.done-mark.rise,
.done-title.rise { transform: translateY(24px); opacity: 0; }
.ready .done-mark.rise,
.ready .done-title.rise {
  animation: fadeUp 0.9s cubic-bezier(.2, .9, .25, 1) both;
  animation-delay: var(--d, 0s);
}

/* ─── адаптив ─── */
@media (max-width: 1200px) {
  .left  { padding: 48px 40px 40px 72px; }
  .right { padding: 40px 72px 40px 40px; }
  .title { font-size: clamp(64px, 9.2vw, 132px); }
  .ghost { font-size: clamp(280px, 40vw, 460px); }
}

@media (max-width: 960px) {
  .page {
    grid-template-rows: auto auto auto auto;
    height: auto;
    min-height: 100vh;
  }
  .body {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto;
  }
  .left {
    padding: 48px 40px 56px;
    border-bottom: 1px solid var(--hair);
  }
  .right {
    padding: 56px 40px;
    align-items: stretch;
  }
  .left::before { background: none; }

  .divider {
    left: 24px;
    right: 24px;
    top: 0;
    bottom: auto;
    width: auto;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--accent), transparent);
    transform-origin: left center;
  }
  .ready .divider { animation: lineDrop 1.2s cubic-bezier(.2, .9, .25, 1) both; }
  @keyframes lineDrop {
    from { transform: scaleX(0); }
    to   { transform: scaleX(1); }
  }
  .ghost { display: none; }

  /* на мобильном фото затемняется сверху вниз */
  .shade {
    background:
      linear-gradient(
        180deg,
        rgba(7, 14, 26, 0.55) 0%,
        rgba(7, 14, 26, 0.5) 50%,
        rgba(7, 14, 26, 0.65) 100%
      ),
      radial-gradient(
        130% 90% at 50% 110%,
        rgba(0, 0, 0, 0) 45%,
        rgba(0, 0, 0, 0.5) 100%
      );
  }
}

@media (max-width: 640px) {
  .masthead,
  .colophon {
    padding: 0 20px;
    font-size: 9px;
    letter-spacing: 0.22em;
  }
  .masthead { flex-wrap: wrap; gap: 8px; padding-top: 14px; padding-bottom: 14px; }
  .masthead .right { margin-left: 0; }

  .left  { padding: 36px 22px 40px; }
  .right { padding: 40px 22px 56px; }

  .title { font-size: 56px; }
  .lead  { font-size: 14px; }

  .meta li,
  .field {
    grid-template-columns: 28px 1fr;
    gap: 10px;
  }
  .meta li a,
  .meta li span:last-child,
  .field input,
  .field textarea {
    grid-column: 1 / -1;
    padding-top: 4px;
  }

  .colophon {
    grid-template-columns: 1fr;
    gap: 4px;
    text-align: left;
    padding: 14px 20px;
  }
  .colophon span { text-align: left !important; }

  .done-title { font-size: 42px; }
}

@media (prefers-reduced-motion: reduce) {
  .photo,
  .ghost,
  .kicker .rule,
  .fade,
  .word.rise,
  .divider,
  .done-mark.rise,
  .done-title.rise {
    animation: none !important;
    transition: none !important;
  }
  .fade,
  .word.rise,
  .done-mark.rise,
  .done-title.rise {
    opacity: 1 !important;
    transform: none !important;
  }
}
</style>