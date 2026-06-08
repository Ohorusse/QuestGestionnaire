<script setup>
import QuestCard from './QuestCard.vue'

defineProps({
  quests: {
    type: Array,
    required: true
  }
})
</script>

<template>

  <div>
    <QuestCard v-for="quest in quests" :key="quest.id" :quest="quest" />
  </div>
</template>


