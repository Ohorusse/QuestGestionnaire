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
  padding: 18px 14px 12px;
  border-radius: 22px;
  background: linear-gradient(180deg, rgba(255, 247, 233, 0.92), rgba(236, 216, 179, 0.82));
  border: 1px solid rgba(122, 82, 31, 0.18);
  box-shadow: 0 12px 28px rgba(88, 57, 21, 0.09);
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
  color: #4d2f10;
}

.quest-list__count {
  margin: 0;
  opacity: 0.7;
  color: #7a5b33;
}

.quest-list__empty {
  margin: 0 4px 12px;
  padding: 14px;
  border: 1px dashed rgba(122, 82, 31, 0.28);
  border-radius: 14px;
  background: rgba(255, 250, 241, 0.7);
  color: #7b674b;
  opacity: 0.95;
}
</style>


