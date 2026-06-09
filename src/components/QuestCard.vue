<script>
export default {
  name: 'QuestCard',
  props: {
    quest: {
      type: Object,
      required: true
    }
  }
}
</script>

<template>
  <div class="quest-card">
      <h2>{{ quest.title }} ({{ quest.difficulty }})</h2>
      <p>{{ quest.description }}</p>
      <p>Reward: {{ quest.reward }}</p>
      <h2 class="centered">Statut: {{ quest.status }}</h2>
  </div>
</template>

<style>
.quest-card {
  border: 1px solid #ccc;
  padding: 16px;
  margin: 16px;
  border-radius: 8px;
  font-family: Papyrus, fantasy;
}

.centered {
  text-align: center;
}
</style>
