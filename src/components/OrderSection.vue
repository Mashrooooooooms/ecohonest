<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const ACCESS_KEY = 'b1130200-2539-44ae-8011-1513b5efff9e'

const name = ref('')
const phone = ref('')
const topic = ref('')
const sent = ref(false)
const sending = ref(false)
const error = ref('')

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

async function submit() {
  sending.value = true
  error.value = ''

  const res = await fetch('https://api.web3forms.com/submit', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    },
    body: JSON.stringify({
      access_key: ACCESS_KEY,
      subject: `Заявка от ${name.value || 'без имени'}`,
      from_name: 'Ecohonest — сайт',
      name: name.value,
      phone: phone.value,
      topic: topic.value
    })
  })

  const data = await res.json()
  sending.value = false

  if (data.success) {
    sent.value = true
    name.value = ''
    phone.value = ''
    topic.value = ''
  } else {
    error.value = data.message || 'Не удалось отправить. Попробуйте ещё раз.'
  }
}

function reset() {
  sent.value = false
  error.value = ''
}
</script>

<template>
  <section ref="root" class="contact" :class="{ 'is-visible': visible }">
    <div class="photo"></div>
    <div class="shade"></div>

    <header class="top">
      <span class="pg">05</span>
      <span class="rule"></span>
      <span class="kicker">Заявка · Консультация · Связь</span>
    </header>

    <main class="body">
      <div class="left">
        <span class="ghost" aria-hidden="true">05</span>

        <div class="stack">
          <p class="kicker-small">
            <span class="rule-line"></span>
            <span>Заявка / Consultation</span>
          </p>

          <h1 class="title">
            <span class="mask">
              <span class="word rise">Оставьте</span>
            </span>
            <span class="mask">
              <span class="word rise word--accent">заявку</span>
            </span>
          </h1>

          <p class="lead">
            Первая консультация бесплатна. Ответим
            в&nbsp;течение рабочего дня и&nbsp;предложим сроки.
          </p>
        </div>

        <ul class="meta">
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
        <div class="divider"></div>

        <form v-if="!sent" class="form" @submit.prevent="submit">
          <div class="form-top">
            <span>Заполните форму</span>
            <span class="cnt">01 / 01</span>
          </div>

          <label class="field">
            <span class="f-num">01</span>
            <span class="f-label">Имя</span>
            <input v-model="name" type="text" placeholder="—" required>
          </label>

          <label class="field">
            <span class="f-num">02</span>
            <span class="f-label">Телефон</span>
            <input v-model="phone" type="tel" placeholder="—" required>
          </label>

          <label class="field field--area">
            <span class="f-num">03</span>
            <span class="f-label">Задача</span>
            <textarea v-model="topic" rows="2" placeholder="—"></textarea>
          </label>

          <button class="send" type="submit" :disabled="sending">
            <span class="send-bg"></span>
            <span class="send-text">{{ sending ? 'Отправка…' : 'Отправить заявку' }}</span>
            <span class="send-arrow">→</span>
          </button>

          <p v-if="error" class="error">{{ error }}</p>

          <p class="note">
            Отправка = согласие с&nbsp;обработкой персональных данных.
          </p>
        </form>

        <div v-else class="done">
          <span class="done-mark">✓</span>
          <p class="done-title">Заявка<br>принята</p>
          <p class="done-sub">
            Свяжемся с&nbsp;вами в&nbsp;течение рабочего дня.
          </p>
          <button class="send send--alt" type="button" @click="reset">
            <span class="send-bg"></span>
            <span class="send-text">Новая заявка</span>
            <span class="send-arrow">→</span>
          </button>
        </div>
      </div>
    </main>

    <span class="v-mark">Ecohonest · 2026</span>
  </section>
</template>

<style scoped>
.contact {
  --bg:      #070e1a;
  --fg:      #f0f5fc;
  --muted:   rgba(240, 245, 252, 0.7);
  --faint:   rgba(240, 245, 252, 0.38);
  --hair:    rgba(240, 245, 252, 0.16);
  --accent:  #6a97ff;
  --accent-d: rgba(106, 151, 255, 0.35);

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

/* ─── шапка ─── */
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

.pg { font-size: 11px; letter-spacing: 0.2em; color: var(--accent); }

.rule {
  flex: 1;
  height: 1px;
  background: rgba(240, 245, 252, 0.4);
  transform-origin: left center;
}

.kicker {
  font-size: 10px;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(240, 245, 252, 0.9);
}

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

.kicker-small {
  display: flex;
  align-items: center;
  gap: 16px;
  margin: 0;
  font-family: system-ui, -apple-system, sans-serif;
  font-size: 10px;
  letter-spacing: 0.44em;
  text-transform: uppercase;
  color: var(--muted);
  text-shadow: 0 1px 16px rgba(0, 0, 0, 0.9);
}

.rule-line {
  display: block;
  width: 60px;
  height: 1px;
  background: var(--accent);
  transform-origin: left center;
  box-shadow: 0 0 12px rgba(106, 151, 255, 0.55);
}

.title {
  margin: 0;
  font-size: clamp(72px, 8.6vw, 168px);
  font-weight: 400;
  line-height: 0.84;
  letter-spacing: -0.055em;
  color: var(--fg);
  margin-left: -0.055em;
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
  font-style: normal;
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
  font-family: system-ui, -apple-system, sans-serif;
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
  transform: scaleY(0);
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
  font-family: system-ui, -apple-system, sans-serif;
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
  font-family: system-ui, -apple-system, sans-serif;
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
  font-family: system-ui, -apple-system, sans-serif;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.36em;
  text-transform: uppercase;
  cursor: pointer;
  overflow: hidden;
  isolation: isolate;
  text-shadow: 0 1px 14px rgba(0, 0, 0, 0.9);
  transition: color 0.4s ease, border-color 0.4s ease, opacity 0.3s ease;
}

.send:disabled {
  cursor: wait;
  opacity: 0.6;
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
.send:hover:not(:disabled) {
  color: #061224;
  border-color: var(--accent);
  text-shadow: none;
}
.send:hover:not(:disabled) .send-bg { transform: translateX(0); }
.send:hover:not(:disabled) .send-arrow { transform: translateX(10px); }

.send-arrow { font-size: 16px; line-height: 1; }
.send--alt { margin-top: 20px; }

.note {
  margin: 18px 0 0;
  font-family: system-ui, -apple-system, sans-serif;
  font-size: 10px;
  letter-spacing: 0.06em;
  line-height: 1.6;
  color: var(--muted);
  text-shadow: 0 1px 12px rgba(0, 0, 0, 0.9);
}

.error {
  margin: 12px 0 0;
  font-family: system-ui, -apple-system, sans-serif;
  font-size: 11px;
  letter-spacing: 0.06em;
  line-height: 1.5;
  color: #ff8a7a;
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
  color: rgba(240, 245, 252, 0.4);
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

@keyframes title-reveal {
  from { opacity: 0; transform: translateY(32px); filter: blur(14px); }
  to   { opacity: 1; transform: translateY(0);    filter: blur(0); }
}

@keyframes rule-grow {
  from { transform: scaleX(0); }
  to   { transform: scaleX(1); }
}

@keyframes line-drop {
  from { transform: scaleY(0); }
  to   { transform: scaleY(1); }
}

@keyframes v-mark-in {
  from { opacity: 0; }
  to   { opacity: 1; }
}

@keyframes word-rise {
  from { transform: translateY(110%); }
  to   { transform: translateY(0); }
}

.contact:not(.is-visible) .top,
.contact:not(.is-visible) .rule,
.contact:not(.is-visible) .kicker,
.contact:not(.is-visible) .kicker-small,
.contact:not(.is-visible) .title,
.contact:not(.is-visible) .word,
.contact:not(.is-visible) .lead,
.contact:not(.is-visible) .meta,
.contact:not(.is-visible) .divider,
.contact:not(.is-visible) .form-top,
.contact:not(.is-visible) .field,
.contact:not(.is-visible) .send,
.contact:not(.is-visible) .note,
.contact:not(.is-visible) .done-mark,
.contact:not(.is-visible) .done-title,
.contact:not(.is-visible) .done-sub,
.contact:not(.is-visible) .v-mark {
  opacity: 0;
}

.contact.is-visible .top          { animation: fade-right 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.05s both; }
.contact.is-visible .rule         { animation: rule-grow 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.25s both; }
.contact.is-visible .kicker       { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.15s both; }

.contact.is-visible .kicker-small { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.35s both; }
.contact.is-visible .rule-line    { animation: rule-grow 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.55s both; }
.contact.is-visible .title        { animation: title-reveal 1.1s cubic-bezier(0.22, 1, 0.36, 1) 0.55s both; }
.contact.is-visible .word         { animation: word-rise 1.15s cubic-bezier(0.2, 0.9, 0.25, 1) 0.65s both; }
.contact.is-visible .mask:nth-of-type(2) .word { animation-delay: 0.80s; }
.contact.is-visible .lead         { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.95s both; }
.contact.is-visible .meta         { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 1.10s both; }

.contact.is-visible .divider      { animation: line-drop 1.2s cubic-bezier(0.22, 1, 0.36, 1) 0.45s both; }
.contact.is-visible .form-top     { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.55s both; }
.contact.is-visible .field:nth-of-type(1) { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.70s both; }
.contact.is-visible .field:nth-of-type(2) { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.80s both; }
.contact.is-visible .field:nth-of-type(3) { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.90s both; }
.contact.is-visible .send         { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 1.05s both; }
.contact.is-visible .note         { animation: fade-up 0.8s cubic-bezier(0.22, 1, 0.36, 1) 1.20s both; }

.contact.is-visible .done-mark    { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.05s both; }
.contact.is-visible .done-title   { animation: title-reveal 1.0s cubic-bezier(0.22, 1, 0.36, 1) 0.15s both; }
.contact.is-visible .done-sub     { animation: fade-up 0.9s cubic-bezier(0.22, 1, 0.36, 1) 0.40s both; }

.contact.is-visible .v-mark       { animation: v-mark-in 1.4s ease-out 0.15s both; }

/* ─── адаптив ─── */
@media (max-width: 1200px) {
  .left  { padding: 48px 40px 40px 72px; }
  .right { padding: 40px 72px 40px 40px; }
  .title { font-size: clamp(64px, 9.2vw, 132px); }
  .ghost { font-size: clamp(280px, 40vw, 460px); }
}

@media (max-width: 960px) {
  .contact {
    grid-template-rows: 60px 1fr;
    height: 100vh;
    height: 100dvh;
  }

  .body {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
    scrollbar-width: none;
  }
  .body::-webkit-scrollbar { display: none; }
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
    transform: scaleY(1) scaleX(0);
  }
  .contact.is-visible .divider {
    animation: rule-grow 1.2s cubic-bezier(0.22, 1, 0.36, 1) 0.45s both;
  }
  .ghost { display: none; }

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

/* ═════════ Мобила (портрет) — форма сверху, контакты снизу ═════════ */

@media (max-width: 768px) and (orientation: portrait) {
  .contact {
    grid-template-rows: 52px 1fr;
  }

  .body {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    overflow: hidden;
    min-height: 0;
    padding: 20px 20px 40px;
    gap: 24px;
  }

  /* ─── форма — наверх ─── */
  .right {
    order: 1;
    padding: 0;
    align-items: stretch;
    flex: 0 0 auto;
  }

  .divider { display: none; }

  /* ─── контакты — вниз ─── */
  .left {
    order: 2;
    padding: 0;
    padding-top: 16px;
    flex: 0 0 auto;
    border-top: 1px solid var(--hair);
    min-height: 0;
  }

  .ghost { display: none; }
  .stack { display: none; }

  /* ─── компактные контакты без подчёркивания ─── */
  .meta {
    max-width: 100%;
    border-top: none;
    gap: 0;
  }

  .meta li {
    grid-template-columns: 20px 80px 1fr;
    gap: 10px;
    padding: 6px 0;
    font-size: 11px;
    align-items: baseline;
  }

  .m-n { font-size: 10px; }
  .m-k { font-size: 9px; letter-spacing: 0.18em; }

  .meta li a,
  .meta li span:last-child {
    font-size: 12px;
  }

  /* снимаем анимированное подчёркивание — на тач-экране не нужен */
  .meta a::after { display: none; }
  .meta a:hover { color: var(--fg); }

  /* ─── форма компактная ─── */
  .form,
  .done {
    max-width: 100%;
  }

  .form-top {
    padding-bottom: 12px;
    margin-bottom: 2px;
    font-size: 9px;
    letter-spacing: 0.28em;
  }

  .field {
    grid-template-columns: 22px 76px 1fr;
    gap: 12px;
    padding: 12px 0;
  }
  .field--area { padding-top: 14px; }

  .f-num { font-size: 11px; }
  .f-label { font-size: 9px; letter-spacing: 0.2em; }

  .field input { font-size: 15px; }
  .field textarea { font-size: 13px; }

  .send {
    margin-top: 20px;
    padding: 16px 18px;
    font-size: 10px;
    letter-spacing: 0.28em;
  }
  .send-arrow { font-size: 14px; }

  .note {
    margin: 12px 0 0;
    font-size: 9px;
    line-height: 1.4;
  }

  .error {
    font-size: 10px;
    margin-top: 8px;
  }

  .done-mark { font-size: 32px; }
  .done-title { font-size: 32px; margin: 4px 0 8px; }
  .done-sub { font-size: 12px; max-width: 100%; }
  .send--alt { margin-top: 14px; }

  .top { padding: 0 20px; gap: 10px; }
  .pg, .kicker { font-size: 9px; }
  .kicker { letter-spacing: 0.2em; }

  .v-mark { display: none; }
}

@media (prefers-reduced-motion: reduce) {
  .photo,
  .ghost,
  .contact.is-visible *,
  .contact.is-visible .mask:nth-of-type(2) .word {
    animation: none !important;
    transition: none !important;
  }
  .contact:not(.is-visible) .top,
  .contact:not(.is-visible) .rule,
  .contact:not(.is-visible) .kicker,
  .contact:not(.is-visible) .kicker-small,
  .contact:not(.is-visible) .title,
  .contact:not(.is-visible) .word,
  .contact:not(.is-visible) .lead,
  .contact:not(.is-visible) .meta,
  .contact:not(.is-visible) .divider,
  .contact:not(.is-visible) .form-top,
  .contact:not(.is-visible) .field,
  .contact:not(.is-visible) .send,
  .contact:not(.is-visible) .note,
  .contact:not(.is-visible) .done-mark,
  .contact:not(.is-visible) .done-title,
  .contact:not(.is-visible) .done-sub,
  .contact:not(.is-visible) .v-mark {
    opacity: 1;
  }
  .contact.is-visible .divider { transform: scaleY(1); }
  .word { transform: none; }
}
</style>