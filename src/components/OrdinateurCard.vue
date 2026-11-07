
<script setup>
defineProps({
  ordinateur: Object
})

const emit = defineEmits(['modifier', 'supprimer'])
</script>

<template>
  <div class="carte">

    <img
      v-if="ordinateur.photo"
      :src="ordinateur.photo"
      :alt="ordinateur.modele"
      class="photo"
    >

    <h3>{{ ordinateur.modele }}</h3>

    <p>Processeur : {{ ordinateur.processeur }}</p>
    <p>Mémoire : {{ ordinateur.memoire }} Go</p>
    <p>Disque : {{ ordinateur.disque }} Go</p>
    <p>Carte graphique : {{ ordinateur.carteGraphique }}</p>

    <button @click="emit('modifier', ordinateur.id)">
      Modifier
    </button>

    <button
      class="supprimer"
      @click="emit('supprimer', ordinateur.id)"
    >
      Supprimer
    </button>

  </div>
</template>

<style scoped>
.carte {
  background-color: white;
  width: 250px;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 8px #ccc;
}

.photo {
  width: 100%;
  height: 150px;
  object-fit: contain;
}

h3 {
  color: #2c3e50;
}

p {
  color: #555;
}

button {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 8px 12px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
}

.supprimer {
  background-color: #e74c3c;
}
</style>
