<script setup>
import { ref } from 'vue'
import HeroSection from './HeroSection.vue'
import AboutSection from './AboutSection.vue'
import ServicesSection from './ServicesSection.vue'
import PainSection from './PainSection.vue'
import OrderSection from './OrderSection.vue'
import ContactsSection from './ContactsSection.vue'

const container = ref(null)
const active = ref(0)

function onScroll(e) {
  active.value = Math.round(e.target.scrollTop / e.target.clientHeight)
}

function goTo(i) {
  container.value.children[i].scrollIntoView({ behavior: 'smooth' })
}
</script>

<template>
  <div class="main">
    <div class="bg-overlay bg-overlay--2" :class="{ 'bg-overlay--on': active >= 2 }"></div>

    <div ref="container" class="scroll" @scroll="onScroll">
      <section class="slide" :class="{ 'slide--dim': active !== 0 }"><HeroSection /></section>
      <section class="slide" :class="{ 'slide--dim': active !== 1 }"><AboutSection /></section>
      <section class="slide" :class="{ 'slide--dim': active !== 2 }"><PainSection :is-active="active === 2" /></section>
      <section class="slide" :class="{ 'slide--dim': active !== 3 }"><ServicesSection :is-active="active === 3" /></section>
      <section class="slide" :class="{ 'slide--dim': active !== 4 }"><OrderSection /></section>
      <section class="slide" :class="{ 'slide--dim': active !== 5 }"><ContactsSection /></section>
    </div>

    <div class="edge edge--top"></div>
    <div class="edge edge--bottom"></div>

    <nav class="nav" :class="`nav--pair${Math.floor(active / 2) + 1}`">
      <button
        v-for="i in 6"
        :key="i"
        class="dot"
        :class="{ active: active === i - 1 }"
        @click="goTo(i - 1)"
      />
    </nav>

    <div class="line top"></div>
    <div class="line bottom"></div>
  </div>
</template>

<style scoped>
.main {
  position: relative;
  height: 100vh;
  height: 100dvh;
  overflow: hidden;
  background-color: #0a1119;
  background-image: url('/bg.webp');
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
}

.bg-overlay {
  position: absolute;
  inset: 0;
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  opacity: 0;
  transition: opacity 1.2s ease;
  pointer-events: none;
  z-index: 0;
}

.bg-overlay--2 { background-image: url('/bg2.webp'); }
.bg-overlay--on { opacity: 1; }

.scroll {
  position: relative;
  z-index: 1;
  height: 100vh;
  height: 100dvh;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
  scrollbar-width: none;
}

.scroll::-webkit-scrollbar { display: none; }

.slide {
  position: relative;
  height: 100vh;
  height: 100dvh;
  scroll-snap-align: start;
  scroll-snap-stop: always;
  overflow: hidden;
  transition: opacity 0.55s ease;
}

.slide--dim { opacity: 0.38; }

/* ─── смягчение стыков между секциями ─── */
.edge {
  position: absolute;
  left: 0;
  right: 0;
  height: 110px;
  z-index: 6;
  pointer-events: none;
}

.edge--top {
  top: 0;
  background: linear-gradient(
    to bottom,
    rgba(5, 10, 16, 0.85) 0%,
    rgba(5, 10, 16, 0.3) 55%,
    rgba(5, 10, 16, 0) 100%
  );
}

.edge--bottom {
  bottom: 0;
  background: linear-gradient(
    to top,
    rgba(5, 10, 16, 0.85) 0%,
    rgba(5, 10, 16, 0.3) 55%,
    rgba(5, 10, 16, 0) 100%
  );
}

.nav {
  position: fixed;
  right: 24px;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  gap: 14px;
  z-index: 20;

  /* цвет активной точки — дефолт (fallback) */
  --dot-active: #a4c47a;
  --dot-active-halo: rgba(164, 196, 122, 0.35);
}

/* пара 1 — Hero + About → зелёный */
.nav--pair1 {
  --dot-active: #a4c47a;
  --dot-active-halo: rgba(164, 196, 122, 0.4);
}

/* пара 2 — Pain + Services → красный */
.nav--pair2 {
  --dot-active: #d14028;
  --dot-active-halo: rgba(209, 64, 40, 0.4);
}

/* пара 3 — Order + Contacts → синий */
.nav--pair3 {
  --dot-active: #4d8bfe;
  --dot-active-halo: rgba(77, 139, 254, 0.4);
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  border: 1px solid rgba(255, 255, 255, 0.5);
  background: transparent;
  padding: 0;
  transition:
    background 0.3s ease,
    border-color 0.3s ease,
    transform 0.2s ease,
    box-shadow 0.3s ease;
}

.dot:hover { border-color: #fff; }

.dot.active {
  background: var(--dot-active);
  border-color: var(--dot-active);
  transform: scale(1.3);
  box-shadow: 0 0 0 4px var(--dot-active-halo);
}

.line {
  position: fixed;
  left: 0;
  right: 0;
  height: 1px;
  background: rgba(255, 255, 255, 0.35);
  z-index: 8;
  pointer-events: none;
}

.top { top: 50px; }
.bottom { bottom: 50px; }

/* ═════════ Mobile ═════════ */

@media (max-width: 768px) {
  .nav { right: 10px; gap: 10px; }
  .dot { width: 6px; height: 6px; }

  .line { opacity: 0.5; }
  .top { top: 10px; }
  .bottom { bottom: 10px; }
}

@media (prefers-reduced-motion: reduce) {
  .slide,
  .bg-overlay {
    transition: none !important;
  }
}
</style>