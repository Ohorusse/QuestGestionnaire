<script setup>
import QuestCard from './QuestCard.vue'

defineProps({
  quests: {
    type: Array,
    required: true
  }
})

defineEmits(['edit-quest', 'delete-quest'])
</script>

<template>

  <div>
    <QuestCard
      v-for="quest in quests"
      :key="quest.id"
      :quest="quest"
      @edit-quest="$emit('edit-quest', quest)"
      @delete-quest="$emit('delete-quest', quest)"
    />
  </div>
</template>


