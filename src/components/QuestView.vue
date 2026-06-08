<script>

import QuestList from './QuestList.vue'

export default {
  components: {
    QuestList
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
        ]
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
  }
}


</script>
<template>
<h1>Table des quêtes</h1>
<div1 class="quest-columns">
  <QuestList title="Quêtes en cours" :quests="questsEC"/>
  <QuestList title="Quêtes disponibles" :quests="questsAF"/>
  <QuestList title="Quêtes terminées" :quests="questsFin"/>
</div1>
</template>

<style>
.quest-columns {
  display: flex;
  gap: 20px;
}
</style>
