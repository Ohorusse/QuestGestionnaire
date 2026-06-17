<script>
import QuestView from './components/QuestView.vue'

export default {
  name: 'App',
  components: { QuestView },
}

</script>
<template>
  <QuestView />
</template>

<style scoped>
:global(body) {
  margin: 0;
  min-width: 320px;
  font-family: 'Cormorant Garamond', Georgia, 'Times New Roman', serif;
  color: #2d2216;
  background:
    radial-gradient(circle at top left, rgba(168, 124, 69, 0.2), transparent 30%),
    radial-gradient(circle at top right, rgba(104, 72, 33, 0.16), transparent 26%),
    linear-gradient(180deg, #f4e6cf 0%, #ead4b0 48%, #d1b187 100%);
}

:global(body::before) {
  content: '';
  position: fixed;
  inset: 0;
  pointer-events: none;
  background-image:
    linear-gradient(rgba(255, 255, 255, 0.12) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 255, 255, 0.12) 1px, transparent 1px),
    radial-gradient(circle at 20% 15%, rgba(103, 69, 29, 0.1), transparent 20%),
    radial-gradient(circle at 80% 5%, rgba(103, 69, 29, 0.08), transparent 16%);
  background-size: 56px 56px, 56px 56px, 100% 100%, 100% 100%;
  opacity: 0.22;
}

:global(#app) {
  min-height: 100vh;
  padding: 32px 20px 40px;
  box-sizing: border-box;
}

:global(h1) {
  width: min(1100px, 100%);
  margin: 0 auto 22px;
  padding: 14px 18px;
  font-size: clamp(2rem, 4vw, 3rem);
  line-height: 1.05;
  letter-spacing: -0.03em;
  color: #4d2f10;
  border: 1px solid rgba(111, 74, 28, 0.18);
  border-radius: 18px;
  background: linear-gradient(180deg, rgba(255, 248, 233, 0.9), rgba(239, 221, 189, 0.78));
  box-shadow: 0 14px 30px rgba(71, 45, 12, 0.08);
}
</style>
