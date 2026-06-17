<script>

import QuestList from './QuestList.vue'

export default {
  components: {
    QuestList
  },
  created() {
    this.loadQuestsFromStorage()
  },
  data() {
    return {
      quests: [

  {
    id: 1,
    title: 'Quête de débutant',
    description: 'Tuez 20 Slimes',
    reward: '10 pièces',
    difficulty: 'Facile',
    status: 'En cours',
  },
  {
    id: 2,
    title: 'Le trésor oublié',
    description: 'Retrouvez le coffre caché dans les ruines anciennes.',
    reward: '50 pièces',
    difficulty: 'Moyenne',
    status: 'Disponible',
  },
  {
    id: 3,
    title: 'Défense du village',
    description: 'Repoussez l’attaque des gobelins sur le village.',
    reward: '100 pièces',
    difficulty: 'Difficile',
    status: 'Terminée',
  }
      ],
      questForm: {
        title: '',
        description: '',
        reward: '',
        difficulty: 'Facile',
      },
      questBeingEditedId: null,
      nextQuestId: 4,
    }
  },

  computed: {
      questsEC() {
        return this.quests.filter(quest => quest.status === 'En cours')
      },
      questsAF() {
        return this.quests.filter(quest => quest.status === 'Disponible')
      },
      questsFin() {
        return this.quests.filter(quest => quest.status === 'Terminée')
      }
    },

  methods:{
    saveQuestsToStorage() {
      localStorage.setItem('questBoardData', JSON.stringify({
        quests: this.quests,
        nextId: this.nextQuestId,
      }))
    },
    loadQuestsFromStorage() {
      const savedData = localStorage.getItem('questBoardData')
      if (!savedData) {
        return
      }

      const parsedData = JSON.parse(savedData)
      if (parsedData.quests) {
        this.quests = parsedData.quests
      }
      if (parsedData.nextId) {
        this.nextQuestId = parsedData.nextId
      }
    },

    addQuest(questToAdd) {
      this.quests.push({
      id: this.nextQuestId,
      ...questToAdd,
      status: 'Disponible',
      })
      this.nextQuestId += 1
      this.saveQuestsToStorage()
    },

    startQuestEdit(quest) {
      this.questBeingEditedId = quest.id
      this.questForm = {
        title: quest.title,
        description: quest.description,
        reward: quest.reward,
        difficulty: quest.difficulty,
      }
    },

    updateQuest(questUpdates) {
      const index = this.quests.findIndex(quest => quest.id === this.questBeingEditedId)

      if (index === -1) {
        return
      }

      this.quests[index] = {
        ...this.quests[index],
        ...questUpdates,
      }

      this.questBeingEditedId = null
      this.questForm = {
        title: '',
        description: '',
        reward: '',
        difficulty: 'Facile',
      }
      this.saveQuestsToStorage()
    },

    changeQuestStatus(questToUpdate, newStatus) {
      const index = this.quests.findIndex(quest => quest.id === questToUpdate.id)

      if (index === -1) {
        return
      }

      this.quests[index].status = newStatus
      this.saveQuestsToStorage()
    },

    deleteQuest(questToRemove) {
      this.quests = this.quests.filter(quest => quest.id !== questToRemove.id)

      if (this.questBeingEditedId === questToRemove.id) {
        this.questBeingEditedId = null
        this.questForm = {
          title: '',
          description: '',
          reward: '',
          difficulty: 'Facile',
        }
      }

      this.saveQuestsToStorage()
    },

    submitQuest() {
      if (!this.questForm.title.trim()) {
        return
      }

      if (this.questBeingEditedId !== null) {
        this.updateQuest({ ...this.questForm })
        return
      }

      this.addQuest({ ...this.questForm })

      this.questForm = {
        title: '',
        description: '',
        reward: '',
        difficulty: 'Facile',
      }
    }
  }
}


</script>

<template>
<h1>Table des quêtes</h1>
<form class="quest-form" @submit.prevent="submitQuest">
  <p class="form-label">{{ questBeingEditedId !== null ? 'Modifier la quête' : 'Ajouter une quête' }}</p>
  <input v-model="questForm.title" type="text" placeholder="Titre de la quête" />
  <textarea v-model="questForm.description" placeholder="Description"></textarea>
  <input v-model="questForm.reward" type="text" placeholder="Récompense" />
  <select v-model="questForm.difficulty">
    <option>Facile</option>
    <option>Moyenne</option>
    <option>Difficile</option>
  </select>
  <button type="submit">{{ questBeingEditedId !== null ? 'Modifier la quête' : 'Ajouter la quête' }}</button>
</form>
<div class="quest-columns">
  <QuestList title="Quêtes en cours" :quests="questsEC" @edit-quest="startQuestEdit" @delete-quest="deleteQuest" @change-status="changeQuestStatus"/>
  <QuestList title="Quêtes disponibles" :quests="questsAF" @edit-quest="startQuestEdit" @delete-quest="deleteQuest" @change-status="changeQuestStatus"/>
  <QuestList title="Quêtes terminées" :quests="questsFin" @edit-quest="startQuestEdit" @delete-quest="deleteQuest" @change-status="changeQuestStatus"/>
</div>
</template>

<style>
.quest-columns {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 20px;
  width: min(1100px, 100%);
  margin: 0 auto;
  align-items: start;
}

.quest-form {
  display: flex;
  flex-direction: column;
  gap: 12px;
  width: min(520px, 100%);
  margin: 0 auto 28px;
  padding: 20px;
  border: 1px solid rgba(122, 82, 31, 0.24);
  border-radius: 20px;
  background: linear-gradient(180deg, rgba(255, 249, 236, 0.96), rgba(240, 221, 188, 0.86));
  box-shadow: 0 14px 32px rgba(88, 57, 21, 0.12);
}

.form-label {
  margin: 0;
  font-weight: bold;
  color: #5c3a16;
}

.quest-form input,
.quest-form textarea,
.quest-form select {
  padding: 12px 14px;
  border: 1px solid rgba(112, 76, 30, 0.22);
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.86);
  color: #3b2a18;
  font: inherit;
  transition: border-color 0.2s ease, box-shadow 0.2s ease, transform 0.2s ease;
}

.quest-form input:focus,
.quest-form textarea:focus,
.quest-form select:focus {
  outline: none;
  border-color: #8c5a1a;
  box-shadow: 0 0 0 4px rgba(140, 90, 26, 0.18);
}

.quest-form button {
  align-self: flex-start;
  padding: 12px 18px;
  border: none;
  border-radius: 12px;
  background: linear-gradient(135deg, #8c5a1a, #c08a39);
  color: #fff8ee;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 10px 18px rgba(140, 90, 26, 0.18);
  transition: transform 0.2s ease, box-shadow 0.2s ease, filter 0.2s ease;
}

.quest-form button:hover {
  transform: translateY(-1px);
  filter: brightness(1.03);
  box-shadow: 0 14px 22px rgba(140, 90, 26, 0.24);
}

.quest-form textarea {
  min-height: 100px;
  resize: vertical;
}

@media (max-width: 1100px) {
  .quest-columns {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 720px) {
  .quest-columns {
    grid-template-columns: 1fr;
  }

  .quest-form {
    width: 100%;
  }

  .quest-form button {
    width: 100%;
  }
}
</style>
