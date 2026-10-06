<script setup>
</script>

<template>
  <section class="hero">
    <div class="frame">
      <header class="top">
        <span class="page">01</span>
        <span class="rule"></span>
        <span class="kicker">Экология · Аудит · Сопровождение</span>
      </header>

      <div class="note note--air">
        <span class="dot"></span>
        <span class="line"></span>
        <div class="body">
          <span class="label">Полный цикл</span>
          <span class="text">Вода · Воздух · Отходы — экологическое сопровождение под ключ.</span>
        </div>
      </div>

      <div class="title-group">
        <h1>Eco<span class="accent">Honest</span></h1>
        <p class="subtitle">Честный эколог</p>

        <div class="note note--sub">
          <span class="dot"></span>
          <span class="line"></span>
          <div class="body">
            <span class="label">Опыт</span>
            <span class="text">Более 20 лет на опасных производственных объектах.</span>
          </div>
        </div>
      </div>

 <div class="meta">
  <p class="est">ООО «Честный эколог»</p>
  <p class="tags">ОГРН 1217700015158 · ИНН 9729304028</p>
  <p class="city">+7 (991) 591-77-78</p>
</div>

<p class="copy">© 2026 · info@ecohonest.ru</p>

      <span class="v-mark">Ecohonest · 2026</span>
    </div>
  </section>
</template>

<style scoped>
.hero {
  position: relative;
  height: 100vh;
  padding: 50px;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.05) 0%, rgba(0, 0, 0, 0.45) 100%);
  color: #eef1e8;
  overflow: hidden;
}

.frame {
  position: relative;
  height: 100%;
  padding: 40px 50px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.top {
  display: flex;
  align-items: center;
  gap: 16px;
  font-family: system-ui, sans-serif;
}

.page {
  font-size: 11px;
  letter-spacing: 0.2em;
  color: #a4c47a;
}

.rule {
  flex: 1;
  height: 1px;
  background: rgba(238, 241, 232, 0.5);
}

.kicker {
  font-size: 10px;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: #eef1e8;
}

.note {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 13px;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #a4c47a;
  flex-shrink: 0;
}

.line {
  width: 60px;
  height: 1px;
  background: rgba(164, 196, 122, 0.55);
  flex-shrink: 0;
}

.body {
  display: flex;
  flex-direction: column;
  gap: 1px;
}

.label {
  font-size: 12px;
  line-height: 1.1;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  font-weight: 600;
  color: #eef1e8;
}

.text {
  font-size: 13px;
  line-height: 1.25;
  color: rgba(238, 241, 232, 0.75);
  max-width: 280px;
}

.note--air {
  position: absolute;
  top: 34%;
  left: 42%;
}

.title-group {
  position: absolute;
  left: 50px;
  bottom: 90px;
}

.note--sub {
  margin-top: 22px;
}

h1 {
  font-size: clamp(80px, 12vw, 180px);
  font-weight: 400;
  letter-spacing: -0.05em;
  line-height: 0.9;
  color: #8b8578;
  font-family: Georgia, "Times New Roman", serif;
}

.accent {
  color: #a4c47a;
}

.subtitle {
  margin-top: 8px;
  font-size: 14px;
  letter-spacing: 0.32em;
  text-transform: uppercase;
  color: #eef1e8;
  font-weight: 500;
}

.meta {
  position: absolute;
  right: 50px;
  bottom: 90px;
  text-align: right;
}

.est,
.city {
  font-size: 12px;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: rgba(238, 241, 232, 0.9);
}

.tags {
  margin-top: 6px;
  font-size: 12px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: rgba(238, 241, 232, 0.65);
}

.city {
  margin-top: 6px;
  color: #a4c47a;
}

.v-mark {
  position: absolute;
  left: 8px;
  top: 50%;
  transform: rotate(-90deg) translateX(-50%);
  transform-origin: left center;
  font-family: system-ui, sans-serif;
  font-size: 10px;
  letter-spacing: 0.35em;
  text-transform: uppercase;
  color: rgba(238, 241, 232, 0.4);
  white-space: nowrap;
}
</style>