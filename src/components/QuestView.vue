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
  display: flex;
  gap: 20px;
}

.quest-form {
  display: flex;
  flex-direction: column;
  gap: 12px;
  max-width: 420px;
  margin-bottom: 24px;
}

.form-label {
  margin: 0;
  font-weight: bold;
}

.quest-form input,
.quest-form textarea,
.quest-form select {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

.quest-form textarea {
  min-height: 100px;
  resize: vertical;
}
</style>
