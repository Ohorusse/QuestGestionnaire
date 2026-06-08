<script>
import QuestView from './components/QuestView.vue'

export default {
  name: 'App',
  components: { QuestView },
  data() {
    return {
      quests: []
    }
  },
}

</script>
<template>
  <QuestView />
</template>

<style scoped></style>
