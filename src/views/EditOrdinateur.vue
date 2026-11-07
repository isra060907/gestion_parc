
<script setup>
import { ref } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

const id = route.params.id

const liste = JSON.parse(localStorage.getItem('ordinateurs')) || []

const ordinateur = ref({
  modele: '',
  processeur: '',
  memoire: '',
  disque: '',
  carteGraphique: '',
  photo: ''
})

// Récupérer les anciennes valeurs
if (id) {
  const ord = liste.find(o => o.id == id)

  if (ord) {
    ordinateur.value = { ...ord }
  }
}

// Choisir une image
function choisirImage(event) {
  const fichier = event.target.files[0]

  if (fichier) {
    const lecteur = new FileReader()

    lecteur.onload = () => {
      ordinateur.value.photo = lecteur.result
    }

    lecteur.readAsDataURL(fichier)
  }
}

// Enregistrer
function enregistrer() {

  if (
    ordinateur.value.modele.trim() === '' ||
    ordinateur.value.processeur.trim() === ''
  ) {
    alert('Le modèle et le processeur sont obligatoires')
    return
  }

  if (
    Number(ordinateur.value.memoire) <= 0 ||
    Number(ordinateur.value.disque) <= 0
  ) {
    alert('La mémoire et le disque doivent être positifs')
    return
  }

  if (id) {
    // Modification
    const index = liste.findIndex(o => o.id == id)

    if (index !== -1) {
      liste[index] = { ...ordinateur.value }
    }

  } else {
    // Ajout
    const nouveau = {
      id: liste.length > 0
        ? Math.max(...liste.map(o => o.id)) + 1
        : 1,
      ...ordinateur.value
    }

    liste.push(nouveau)
  }

  localStorage.setItem('ordinateurs', JSON.stringify(liste))

  alert('Enregistrement effectué')

  router.push('/ordinateurs')
}

// Annuler
function annuler() {
  router.push('/ordinateurs')
}
</script>

<template>
  <div class="container">

    <h1>{{ id ? 'Modifier un ordinateur' : 'Ajouter un ordinateur' }}</h1>

    <form @submit.prevent="enregistrer">

      <div class="modifier-container">

        <!-- Partie image -->
        <div class="image-pc">

          <div class="cadre-image">

            <img
              v-if="ordinateur.photo"
              :src="ordinateur.photo"
              :alt="ordinateur.modele"
            >

            <p v-else>Aucune image</p>

          </div>

          <label class="choisir">
            Choisir une image
            <input
              type="file"
              accept="image/*"
              @change="choisirImage"
              hidden
            >
          </label>

          <p class="info">
            (ou indiquer un nom de fichier)
          </p>

          <input
            type="text"
            v-model="ordinateur.photo"
            placeholder="Photo (URL ou chemin)"
          >

        </div>

        <!-- Partie formulaire -->
        <div class="formulaire">

          <label>Modèle *</label>
          <input
            type="text"
            v-model="ordinateur.modele"
            required
          >

          <label>Processeur *</label>
          <input
            type="text"
            v-model="ordinateur.processeur"
            required
          >

          <div class="ligne">

            <div>
              <label>Mémoire (Go) *</label>
              <input
                type="number"
                v-model.number="ordinateur.memoire"
                min="1"
                required
              >
            </div>

            <div>
              <label>Disque (Go) *</label>
              <input
                type="number"
                v-model.number="ordinateur.disque"
                min="1"
                required
              >
            </div>

          </div>

          <label>Carte graphique *</label>
          <input
            type="text"
            v-model="ordinateur.carteGraphique"
          >

        </div>

      </div>

      <!-- Boutons -->
      <div class="actions">

        <button
          type="button"
          class="annuler"
          @click="annuler"
        >
          Annuler
        </button>

        <button type="submit" class="enregistrer">
          Enregistrer
        </button>

      </div>

    </form>

  </div>
</template>

<style scoped>
.container {
  max-width: 950px;
  margin: 40px auto;
  padding: 25px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 2px 8px #ccc;
}

h1 {
  background-color: #1976d2;
  color: white;
  padding: 15px;
  text-align: center;
  border-radius: 5px;
  margin-bottom: 25px;
}

.modifier-container {
  display: flex;
  gap: 25px;
  align-items: flex-start;
}

/* Image */
.image-pc {
  width: 40%;
  text-align: center;
}

.cadre-image {
  width: 100%;
  height: 230px;
  border: 1px solid #ddd;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 15px;
}

.cadre-image img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.choisir {
  display: inline-block;
  background-color: #e5e9ed;
  padding: 12px 25px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
}

.choisir:hover {
  background-color: #d0d7de;
}

.info {
  color: #777;
  font-size: 13px;
}

/* Formulaire */
.formulaire {
  width: 60%;
}

.formulaire label {
  display: block;
  margin-top: 10px;
  font-weight: bold;
  color: #333;
}

input[type="text"],
input[type="number"] {
  width: 100%;
  box-sizing: border-box;
  padding: 10px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.ligne {
  display: flex;
  gap: 15px;
}

.ligne div {
  width: 50%;
}

/* Boutons */
.actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 25px;
}

button {
  padding: 12px 20px;
  border: none;
  border-radius: 5px;
  color: white;
  cursor: pointer;
}

.enregistrer {
  background-color: #0866d8;
}

.annuler {
  background-color: #7f8c8d;
}

button:hover {
  opacity: 0.85;
}

/* Responsive */
@media (max-width: 650px) {
  .modifier-container {
    flex-direction: column;
  }

  .image-pc,
  .formulaire {
    width: 100%;
  }
}
</style>
