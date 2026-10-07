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

    <div class="line top"></div>

    <div ref="container" class="scroll" @scroll="onScroll">
      <section class="slide"><HeroSection /></section>
      <section class="slide"><AboutSection /></section>
      <section class="slide"><PainSection :is-active="active === 2" /></section>
      <section class="slide"><ServicesSection :is-active="active === 3" /></section>
      <section class="slide"><OrderSection /></section>
      <section class="slide"><ContactsSection /></section>
    </div>

    <nav class="nav">
      <button
        v-for="i in 6"
        :key="i"
        class="dot"
        :class="{ active: active === i - 1 }"
        @click="goTo(i - 1)"
      />
    </nav>

    <div class="line bottom"></div>
  </div>
</template>

<style scoped>
.main {
  position: relative;
  height: 100vh;
  overflow: hidden;
  background: url('/bg.webp') center / cover no-repeat;
}

.bg-overlay {
  position: absolute;
  inset: 0;
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  opacity: 0;
  transition: opacity 1s ease;
  pointer-events: none;
  z-index: 0;
}

.bg-overlay--2 { background-image: url('/bg2.webp'); }
.bg-overlay--on { opacity: 1; }

.scroll {
  position: relative;
  z-index: 1;
  height: 100vh;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
  scrollbar-width: none;
}

.scroll::-webkit-scrollbar { display: none; }

.slide {
  height: 100vh;
  scroll-snap-align: start;
  scroll-snap-stop: always;
  position: relative;
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
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  border: 1px solid rgba(255, 255, 255, 0.5);
  background: transparent;
  padding: 0;
  transition: background 0.2s, border-color 0.2s, transform 0.2s;
}

.dot:hover { border-color: #fff; }

.dot.active {
  background: #4d8bfe;
  border-color: #4d8bfe;
  transform: scale(1.3);
}

.line {
  position: fixed;
  left: 0;
  right: 0;
  height: 1px;
  background: rgba(255, 255, 255, 0.35);
  z-index: 10;
  pointer-events: none;
}

.top { top: 50px; }
.bottom { bottom: 50px; }
</style>