<script setup>
import QuestCard from './QuestCard.vue'

defineProps({
  title: {
    type: String,
    required: true
  },
  quests: {
    type: Array,
    required: true
  }
})

defineEmits(['edit-quest', 'delete-quest', 'change-status'])
</script>

<template>

  <div class="quest-list">
    <div class="quest-list__header">
      <slot name="header">
        <h2>{{ title }}</h2>
      </slot>
      <p class="quest-list__count">{{ quests.length }} quête(s)</p>
    </div>

    <p v-if="quests.length === 0" class="quest-list__empty">Aucune quête ici.</p>

    <QuestCard
      v-for="quest in quests"
      :key="quest.id"
      :quest="quest"
      @edit-quest="$emit('edit-quest', quest)"
      @delete-quest="$emit('delete-quest', quest)"
      @change-status="$emit('change-status', quest, $event)"
    />
  </div>
</template>

<style>
.quest-list {
  flex: 1;
  min-width: 280px;
}

.quest-list__header {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  gap: 12px;
  margin: 0 16px 12px;
}

.quest-list__header h2 {
  margin: 0;
}

.quest-list__count {
  margin: 0;
  opacity: 0.7;
}

.quest-list__empty {
  margin: 0 16px 12px;
  padding: 12px;
  border: 1px dashed #ccc;
  border-radius: 8px;
  opacity: 0.75;
}
</style>


