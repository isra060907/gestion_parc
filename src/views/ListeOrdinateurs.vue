<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import hp from '../assets/images/hp.jpg'
import dell from '../assets/images/dell.jpg'
import lenovo from '../assets/images/lenovo.jpg'
import asus from '../assets/images/asus.jpg'
import acer from '../assets/images/acer.jpg'
import OrdinateurCard from '../components/OrdinateurCard.vue'
const router = useRouter()
const exemples = [
  {
    id: 1,
    modele: 'HP Pavilion',
    processeur: 'Intel Core i5',
    memoire: 8,
    disque: 512,
    carteGraphique: 'Intel UHD',
    photo: hp
  },
  {
    id: 2,
    modele: 'Dell Inspiron',
    processeur: 'Intel Core i7',
    memoire: 16,
    disque: 512,
    carteGraphique: 'NVIDIA GTX',
    photo: dell
  },
  {
    id: 3,
    modele: 'Lenovo ThinkPad',
    processeur: 'Intel Core i5',
    memoire: 8,
    disque: 256,
    carteGraphique: 'Intel UHD',
    photo: lenovo
  },
  {
    id: 4,
    modele: 'Asus VivoBook',
    processeur: 'AMD Ryzen 5',
    memoire: 16,
    disque: 1000,
    carteGraphique: 'AMD Radeon',
    photo: asus
  },
  {
    id: 5,
    modele: 'Acer Aspire',
    processeur: 'Intel Core i3',
    memoire: 8,
    disque: 512,
    carteGraphique: 'Intel UHD',
    photo: acer
  }
]
if (localStorage.getItem('ordinateurs') === null) {
  localStorage.setItem('ordinateurs', JSON.stringify(exemples))
}
const liste = ref(
  JSON.parse(localStorage.getItem('ordinateurs'))
)
const recherche = ref('')
const total = computed(() => liste.value.length)
const ordinateursFiltres = computed(() => {
  return liste.value.filter(ord =>
    ord.modele.toLowerCase().includes(recherche.value.toLowerCase()) ||
    ord.processeur.toLowerCase().includes(recherche.value.toLowerCase())
  )
})
function ajouter() {
  router.push('/ordinateurs/ajouter')
}
function modifier(id) {
  router.push('/ordinateurs/modifier/' + id)
}
function supprimer(id) {
  if (confirm('Voulez-vous vraiment supprimer cet ordinateur ?')) {
    liste.value = liste.value.filter(ord => ord.id !== id)
    localStorage.setItem(
      'ordinateurs',
      JSON.stringify(liste.value)
    )
  }
}
function trierParMemoire() {
  liste.value.sort((a, b) => b.memoire - a.memoire)
  localStorage.setItem(
    'ordinateurs',
    JSON.stringify(liste.value)
  )
}
</script>
<template>
  <div class="container">
    <h1>Gestion du parc informatique</h1>

    <p>Nombre d'ordinateurs : {{ total }}</p>
<br><br>
    <button class="ajouter" @click="ajouter">
      Ajouter un ordinateur
    </button>
    <br><br>
    <input
      type="text"
      v-model="recherche"
      placeholder="Rechercher par modèle ou processeur..."
    >

    <br><br>
    <div class="cartes">

      <OrdinateurCard
        v-for="ord in ordinateursFiltres"
        :key="ord.id"
        :ordinateur="ord"
        @modifier="modifier"
        @supprimer="supprimer"
      />

      <p v-if="ordinateursFiltres.length === 0">
        Aucun ordinateur trouvé.
      </p>

    </div>

    <br><br>
    <button class="trier" @click="trierParMemoire">
      Trier par mémoire (décroissant)
    </button>

  </div>
</template>

<style scoped>
.container {
  padding: 30px;
  text-align: center;
}

h1 {
  color: #2c3e50;
}

.ajouter {
  background-color: #27ae60;
  color: white;
  padding: 12px 18px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

input {
  width: 300px;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 14px;
  outline: none;
}

input:focus {
  border-color: #3498db;
}

.cartes {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  margin-top: 25px;
}

button {
  background-color: #3498db;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 8px;
  font-size: 15px;
  font-weight: bold;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background-color: #217dbb;
  transform: translateY(-2px);
}

button:active {
  transform: scale(0.97);
}
</style>
