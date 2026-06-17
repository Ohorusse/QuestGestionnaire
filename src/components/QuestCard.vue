<script>
export default {
  name: 'QuestCard',
  props: {
    quest: {
      type: Object,
      required: true
    }
  },

  emits: ['edit-quest', 'delete-quest', 'change-status'],
  computed: {
    cardClass() {
      if (this.quest.difficulty === 'Difficile') {
        return 'quest-card--hard'
      }

      if (this.quest.difficulty === 'Moyenne') {
        return 'quest-card--medium'
      }

      return 'quest-card--easy'
    },
    canStartQuest() {
      return this.quest.status === 'Disponible'
    },
    canFinishQuest() {
      return this.quest.status === 'En cours'
    }
  }
}
</script>

<template>
  <div class="quest-card" :class="cardClass" @click="$emit('edit-quest')">
      <h2>{{ quest.title }} ({{ quest.difficulty }})</h2>
      <p>{{ quest.description }}</p>
      <p>Récompense: {{ quest.reward }}</p>
      <p v-if="quest.status === 'Terminée'" class="reward-badge">Quête validée</p>
      <p v-else class="reward-badge reward-badge--empty">Pas encore terminée</p>
      <h2 class="centered">Statut: {{ quest.status }}</h2>
      <div class="status-actions">
        <button v-if="canStartQuest" class="status-btn" @click.stop="$emit('change-status', 'En cours')">Commencer</button>
        <button v-if="canFinishQuest" class="status-btn" @click.stop="$emit('change-status', 'Terminée')">Terminer</button>
        <button v-if="canFinishQuest" class="status-btn status-btn--ghost" @click.stop="$emit('change-status', 'Disponible')">Remettre à dispo</button>
      </div>
      <button v-if="quest.status !== 'Terminée'" class="delete-btn" @click.stop="$emit('delete-quest')">Supprimer</button>
      <p v-else class="locked-note">Quête terminée, suppression désactivée.</p>
  </div>
</template>

<style>
.quest-card {
  position: relative;
  margin: 0 0 16px;
  padding: 18px;
  border: 1px solid rgba(122, 82, 31, 0.18);
  border-radius: 18px;
  font-family: Georgia, 'Times New Roman', serif;
  cursor: pointer;
  background: rgba(255, 250, 242, 0.86);
  box-shadow: 0 8px 24px rgba(88, 57, 21, 0.09);
  transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease;
  overflow: hidden;
}

.quest-card::before {
  content: '';
  position: absolute;
  inset: 0 auto auto 0;
  width: 100%;
  height: 4px;
  background: linear-gradient(90deg, #8c5a1a, #c08a39, #8c5a1a);
  opacity: 0.7;
}

.quest-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 14px 32px rgba(88, 57, 21, 0.14);
  border-color: rgba(140, 90, 26, 0.24);
}

.quest-card h2,
.quest-card p {
  margin-top: 0;
}

.quest-card h2 {
  margin-bottom: 10px;
  color: #4d2f10;
}

.quest-card p {
  margin-bottom: 8px;
  color: #5c4630;
}

.quest-card--easy {
  background: linear-gradient(180deg, rgba(241, 245, 233, 0.96), rgba(255, 250, 242, 0.86));
}

.quest-card--medium {
  background: linear-gradient(180deg, rgba(250, 238, 210, 0.96), rgba(255, 250, 242, 0.86));
}

.quest-card--hard {
  background: linear-gradient(180deg, rgba(247, 224, 214, 0.97), rgba(255, 250, 242, 0.86));
}

.reward-badge {
  font-weight: bold;
  color: #7a5b33;
}

.reward-badge--empty {
  opacity: 0.7;
  color: #7b674b;
}

.delete-btn {
  margin-top: 12px;
  padding: 9px 14px;
  border: none;
  border-radius: 10px;
  background: linear-gradient(135deg, #8d3a28, #b14f39);
  color: #fff8ee;
  font-weight: 700;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease, filter 0.2s ease;
}

.status-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 12px;
}

.status-btn {
  padding: 9px 14px;
  border: none;
  border-radius: 10px;
  background: linear-gradient(135deg, #8c5a1a, #c08a39);
  color: #fff8ee;
  font-weight: 700;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease, filter 0.2s ease;
}

.status-btn--ghost {
  background: linear-gradient(135deg, #6f5b41, #8a7458);
}

.delete-btn:hover,
.status-btn:hover {
  transform: translateY(-1px);
  filter: brightness(1.03);
}

.delete-btn:hover {
  box-shadow: 0 10px 18px rgba(141, 58, 40, 0.18);
}

.status-btn:hover {
  box-shadow: 0 10px 18px rgba(140, 90, 26, 0.18);
}

.locked-note {
  margin-top: 12px;
  font-style: italic;
  opacity: 0.8;
  color: #7b674b;
}

.centered {
  text-align: center;
  color: #4d2f10;
}
</style>
