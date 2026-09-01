<template>
  <h1> Correction :</h1>
  <div class="contain">
    <div class="contain2">
      <div class="score">
        <div >
          <h3 class="h31">Votre note : {{ score }}/{{ props.page.questions.length }}</h3>
          <h3 v-if="score >= 26 ">Bravo vous ete dans la moyenne !</h3>
          <h3 v-if="score <26 ">Votre score ne vaut pas 26 c'est très mauvais</h3>
          <h3 v-if="score === 40 ">Excellent vous avez valider toutes les questions !</h3>
          </div>
      </div>
    </div>
  </div>
  <h2>Vos reponses corrigé :</h2>
  <ul>
    <li
      v-for="(reponse, index) in props.reponses"
      :key="index"
      :class="
        memeReponse(props.page.questions[index].correct_answer, reponse)
          ? 'bonne'
          : 'mauvaise'
      "
    >
     Question {{ index + 1 }} : {{ reponse }}
    </li>
  </ul>
</template>

<script setup>
import { computed, ref } from "vue";

const props = defineProps({
  reponses: Array,
  etape: Number,
  page: Object,
});



const memeReponse = (a, b) => {
  if (!Array.isArray(a) || !Array.isArray(b)) {
    return a === b;
  }

  return a.length === b.length && a.every((reponse) => b.includes(reponse));
};

const score = computed(() => {
  return props.page.questions.reduce((acc, question, k) => {
    if (memeReponse(question.correct_answer, props.reponses[k])) {
      return acc + 1;
    }

    return acc;
  }, 0);
}); 
</script>




<style scoped>
.h3{
    padding-bottom: 20px;
    padding-top: 20px;

}
.score {

  margin: 50px 0 50px 0;
  border: solid white;
  border-radius: 20px;
  padding: 20px;
}

.bonne {
  border: solid white;
  border-radius: 10px;
  padding: 10px;
  background: green;
  color: white;
  font-weight: bold;
  margin-right: 10px;
}
.mauvaise {
  border: solid white;
  border-radius: 10px;
  padding: 10px;
  background: red;
  color: white;
  font-weight: bold;
  margin-right: 10px;
}

li {
  list-style: none;
}
</style>
