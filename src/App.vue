<template>
  <div class="svg" @click="changement" v-if="etat === 'question' || etat === 'recap'"><svg width="24" height="24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
 <path d="M3 12h18"></path>
 <path d="M3 6h18"></path>
 <path d="M3 18h18"></path>
</svg></div>
<div class="listeSerie" v-if="estVisible">
    <h2>Vos serie</h2>
      <button v-for="serie in series" :key="serie.title" @click="choisirSerie2(serie)">
      {{ serie.title }}
    </button>
</div>

  <div v-if="etat === 'selection'" class="container premierePage">
    <h1>Choisissez une série</h1>
    <div class="choix">
    <button v-for="serie in series" :key="serie.title" @click="choisirSerie(serie)">
      {{ serie.title }}
    </button>
    </div>
  </div>

  <div v-if="etat === 'question' || etat === 'recap'" class="container deuxiemePage" @click="effacerListe" >
    <h1>{{ page.title }}</h1>
    <Image :key="page.title" :page="page"/>
  </div>

  <div v-if="etat === 'error'"><p class="red">Impossible de charger le diapo</p></div>
  <p :aria-busy="etat === 'chargement'"></p>

</template>



<script setup>
import { onMounted, ref } from "vue"
import Image from "./composants/images.vue"

const estVisible = ref(false)
const effacer = ref(false)
const series = ref([])
const page = ref(null)
const etat = ref('chargement')

onMounted(() => {
  fetch('/questions.json')
    .then(r => {
      if(r.ok){
        return r.json()
      }
      else{
        throw new Error('Impossible de charger le Diapo')
      }
    })
    .then(data => {
      series.value = data
      etat.value = 'selection'
    })
    .catch(e =>
      etat.value = 'error'
    )
})


const changement = () => {
  estVisible.value = !estVisible.value
}

const effacerListe = () => {
  estVisible.value = false
}

function choisirSerie(serie) {
  page.value = serie
  etat.value = 'question'
}


function choisirSerie2(serie) {
  page.value = serie
  etat.value = 'question'
  changement()
}

function changerSerie() {
  page.value = null
  etat.value = 'selection'
}

function changerSeri() {
  etat.value = 'choixDeserie'
}

</script>

<style >

.red{
  color: red;
}

h1{
  margin-top: 30px;
}

.container{
  width:100% ;
}


.listeSerie{
  position: fixed;
  top: 60px;
  left: 30px;
  border-radius: 20px;
  border: solid 1px rgb(1, 114, 173);
  display: grid;
  width: 50vw;
  height: 500px;
  overflow-y: scroll;
  z-index: 10;
  background: rgb(19, 23, 31,);
  opacity: 0.9999;
  backdrop-filter: blur(10px);
}

.listeSerie h2{
  margin-left: 10px;
  margin-top: 10px;
}

.listeSerie button{
  background: rgb(19, 23, 31);
  opacity: 0.8;
  color: rgb(1, 114, 173);
  transition: .8s;
}

.listeSerie button:hover{
  background:rgb(1, 114, 173);
  opacity: 0.5;
  color:white;
}


.svg{  
  background:rgb(19, 23, 31, 0.9);
  opacity: 0.9;
  backdrop-filter: blur(10px);
  width: 100vw;
  display: flex;
  top: 0px;
  height: 55px;
  border-bottom: solid 0.5px rgb(210, 217, 221, 0.5);
  position: fixed;
}

.svg svg{
  margin-top: 17px ;
  margin-left: 25px;
  color: white;
}
.choix{
  display: grid;
  gap: 20px;
}

.premierePage{
  margin-top: 50px;
  text-align: center;
}

.premierePage button{
  opacity: 0.8;
  transition: .8s;
}



.premierePage h1{
  margin-bottom: 20px;


}

.deuxiemePage{
  margin-top: 80px;
  padding-bottom: 100px;
}
 
</style>