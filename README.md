# Gestionnaire de quêtes

Projet Vue.js de TP sur un gestionnaire de quêtes façon RPG.

## Thème choisi

J’ai choisi le thème RPG parce qu’il permet de travailler les composants Vue.js et la logique de board sans passer trop de temps sur le design.

## Fonctionnalités implémentées

- Board avec plusieurs colonnes: quêtes disponibles, en cours et terminées.
- Affichage des quêtes avec `v-for`.
- Ajout d’une quête avec formulaire et `v-model`.
- Modification d’une quête en cliquant sur une carte.
- Suppression d’une quête.
- Déplacement d’une quête entre les colonnes.
- Sauvegarde et chargement dans `localStorage`.
- Rendu conditionnel selon la difficulté et le statut.
- Props et emits pour faire communiquer les composants.
- Slot simple dans la liste des quêtes.

## Lancement du projet

```sh
npm install
npm run dev
```

## Scripts utiles

```sh
npm run build
npm run lint
```
