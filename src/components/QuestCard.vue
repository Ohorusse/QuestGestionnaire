<script>
export default {
  name: 'QuestCard',
  props: {
    quest: {
      type: Object,
      required: true
    }
  },

  emits: ['edit-quest', 'delete-quest'],
  computed: {
    questClass() {
      if (this.quest.difficulty === 'Difficile') {
        return 'quest-card--hard'
      }

      if (this.quest.difficulty === 'Moyenne') {
        return 'quest-card--medium'
      }

      return 'quest-card--easy'
    }
  }
}
</script>

<template>
  <div class="quest-card" :class="questClass" @click="$emit('edit-quest')">
      <h2>{{ quest.title }} ({{ quest.difficulty }})</h2>
      <p>{{ quest.description }}</p>
      <p>Récompense: {{ quest.reward }}</p>
      <p v-if="quest.status === 'Terminée'" class="reward-badge">🏆 Récompense : {{ quest.reward }}</p>
      <p v-else class="reward-badge reward-badge--empty">Pas encore terminée</p>
      <h2 class="centered">Statut: {{ quest.status }}</h2>
      <button v-if="quest.status !== 'Terminée'" class="delete-btn" @click.stop="$emit('delete-quest')">Supprimer</button>
      <p v-else class="locked-note">Quête terminée, suppression désactivée.</p>
  </div>
</template>

<style>
.quest-card {
  border: 1px solid #ccc;
  padding: 16px;
  margin: 16px;
  border-radius: 8px;
  font-family: Papyrus, fantasy;
  cursor: pointer;
}

.quest-card--easy {
  background: #eef8ee;
}

.quest-card--medium {
  background: #fff5db;
}

.quest-card--hard {
  background: #ffe3e3;
}

.reward-badge {
  font-weight: bold;
}

.reward-badge--empty {
  opacity: 0.7;
}

.delete-btn {
  margin-top: 10px;
  padding: 8px 12px;
  border: none;
  border-radius: 6px;
  background: #d9534f;
  color: white;
  cursor: pointer;
}

.locked-note {
  margin-top: 10px;
  font-style: italic;
  opacity: 0.8;
}

.centered {
  text-align: center;
}
</style>
