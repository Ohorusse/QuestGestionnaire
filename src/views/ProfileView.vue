<script setup></script>

const user = [
  {
    id: 1,
    name: 'Test',
    class: 'Guerrier',
    level: 5,
    questsInProgress: 2,
  }
]

<template>
  <div>
    <h1>Profil de l'Aventurier</h1>
    <p>Nom: {{ user.name }}</p>
    <p>Niveau: {{ user.level }}</p>
    <p>Quêtes en cours: {{ user.questsInProgress }}</p>
    <p>Quêtes terminées: 10</p>
  </div>
</template>
