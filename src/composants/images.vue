<template>
  <h3 v-if="etat === 'question'">
    Question : {{ etape + 1 }}/{{ props.page.questions.length }}
  </h3>
  <Progress :value="etape" :max="props.page.questions.length" />
    <div v-if="etat === 'question'" class="timer">
    <h4>Temps restant : {{ tempsRestant }}/{{ tempsTotal }} secondes</h4>

    <div class="timer-bar">
      <div
        class="timer-progress"
        :style="{ width: (tempsRestant / tempsTotal) * 100 + '%' }"
      ></div>
    </div>
  </div>
  <div v-if="etat === 'question'" class="container grid">
    <div class="image">
      <h1 :aria-busy="!chargeImage" v-if="!chargeImage">Chargement de l'image...</h1>
      <img
        :src="`/images/${page.id}/${page.questions[etape].image}`"
        alt="Image question"
        @load="chargeImage = true"
        v-show="chargeImage"
      />
    </div>
    <div>
      <Questions
        class="question"
        :key="etape"
        :page="page"
        :etape="etape"
        :type="type"
        @suivant="ajouterReponse"
        :retirerReponse="retirerReponse"
      />

      <!-- {{ props.page.questions[etape].correct_answer }} -->

    </div>
  </div>


  <Recapitulatif
    v-if="etat === 'recap'"
    :page="page"
    :reponses="reponses"
    :etape="etape"
  />

</template>
<script setup>
import { computed, ref, watch, onUnmounted } from "vue";
import Questions from "./questions.vue";
import Recapitulatif from "./recapitulatif.vue";
import Progress from "./progress.vue";

const chargeImage = ref(false);
const props = defineProps({
  page: Object,
});


const etape = ref(0);
const etat = ref("question");
const reponses = ref([]);

const tempsTotal = 1500;
const tempsRestant = ref(tempsTotal);

let timer = null;


const type = computed(() => {
  return props.page.questions[etape.value].type;
});


const questionSuivante = () => {
  if (etape.value === props.page.questions.length - 1) {
    clearInterval(timer);
    etat.value = "recap";
  } else {
    etape.value++;
  }
};

const questionPrecedente = () => {
    clearInterval(timer)
    etape.value--;
}


const ajouterReponse = (reponse) => {
  clearInterval(timer);

  reponses.value[etape.value ] = reponse;

  questionSuivante();
};


const retirerReponse = (reponse) => {
  clearInterval(timer);
  reponses.value[etape.value] = reponse;

  questionPrecedente();
};


const demarrerTimer = () => {
  tempsRestant.value = tempsTotal;
  clearInterval(timer);
  timer = setInterval(() => {
    tempsRestant.value--;
    if (tempsRestant.value <= 0) {
      clearInterval(timer);
      reponses.value[etape.value] = null;
      questionSuivante();
    }
  }, 1000);
};

watch(
  etape,
  () => {
    if (etat.value === "question") {
      demarrerTimer();
    }
  },
  { immediate: true }
);



onUnmounted(() => {
  clearInterval(timer);
});
</script>





<style scoped>
.container {
   align-items: center;
}

.image {
  margin-top: 30px;
  width: 100%;
  height: auto;
  border-radius: 10px;
  transition: transform 0.9s ease-in-out;
}

img {
  width: 100%;
  height: auto;
  object-fit: cover;
  border-radius: 20px;
}

.question {
  /* margin-left: 40px;
  margin-top: 50px; */

  margin: 0;
}
.timer {
  width: auto;
  margin: 20px auto 0 auto;
  
}

.timer-bar {
  width: 250px;
  height: 10px;
  background: #b50d0d;
  border-radius: 20px;
  overflow: hidden;
}

.timer-progress {
  height: 100%;
  background: #0172ad;
  border-radius: 20px;
  transition: width 1s linear;
}


button{
  margin-right: auto;
  display: block;
  top: 5px;

} 
</style>
