<script>

import QuestList from './QuestList.vue'

export default {
  components: {
    QuestList
  },
  created() {
    this.loadBoardData()
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
      newQuest: {
        title: '',
        description: '',
        reward: '',
        difficulty: 'Facile',
      },
      editingQuestId: null,
      nextId: 4,
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
    saveBoardData() {
      localStorage.setItem('questBoardData', JSON.stringify({
        quests: this.quests,
        nextId: this.nextId,
      }))
    },
    loadBoardData() {
      const savedData = localStorage.getItem('questBoardData')
      if (!savedData) {
        return
      }

      const parsedData = JSON.parse(savedData)
      if (parsedData.quests) {
        this.quests = parsedData.quests
      }
      if (parsedData.nextId) {
        this.nextId = parsedData.nextId
      }
    },

    addQuest(newQuest) {
      this.quests.push({
      id: this.nextId,
      ...newQuest,
      status: 'Disponible',
      })
      this.nextId += 1
      this.saveBoardData()
    },

    startEditQuest(quest) {
      this.editingQuestId = quest.id
      this.newQuest = {
        title: quest.title,
        description: quest.description,
        reward: quest.reward,
        difficulty: quest.difficulty,
      }
    },

    updateQuest(updatedQuest) {
      const index = this.quests.findIndex(quest => quest.id === this.editingQuestId)

      if (index === -1) {
        return
      }

      this.quests[index] = {
        ...this.quests[index],
        ...updatedQuest,
      }

      this.editingQuestId = null
      this.newQuest = {
        title: '',
        description: '',
        reward: '',
        difficulty: 'Facile',
      }
      this.saveBoardData()
    },
    
    deleteQuest(questToDelete) {
      this.quests = this.quests.filter(quest => quest.id !== questToDelete.id)

      if (this.editingQuestId === questToDelete.id) {
        this.editingQuestId = null
        this.newQuest = {
          title: '',
          description: '',
          reward: '',
          difficulty: 'Facile',
        }
      }

      this.saveBoardData()
    },

    submitQuest() {
      if (!this.newQuest.title.trim()) {
        return
      }

      if (this.editingQuestId !== null) {
        this.updateQuest({ ...this.newQuest })
        return
      }

      this.addQuest({ ...this.newQuest })

      this.newQuest = {
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
  <p class="form-label">{{ editingQuestId !== null ? 'Modifier la quête' : 'Ajouter une quête' }}</p>
  <input v-model="newQuest.title" type="text" placeholder="Titre de la quête" />
  <textarea v-model="newQuest.description" placeholder="Description"></textarea>
  <input v-model="newQuest.reward" type="text" placeholder="Récompense" />
  <select v-model="newQuest.difficulty">
    <option>Facile</option>
    <option>Moyenne</option>
    <option>Difficile</option>
  </select>
  <button type="submit">{{ editingQuestId !== null ? 'Modifier la quête' : 'Ajouter la quête' }}</button>
</form>
<div class="quest-columns">
  <QuestList title="Quêtes en cours" :quests="questsEC" @edit-quest="startEditQuest" @delete-quest="deleteQuest"/>
  <QuestList title="Quêtes disponibles" :quests="questsAF" @edit-quest="startEditQuest" @delete-quest="deleteQuest"/>
  <QuestList title="Quêtes terminées" :quests="questsFin" @edit-quest="startEditQuest" @delete-quest="deleteQuest"/>
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
