<template>
<div>
<h2 >{{ props.page.questions[props.etape].question }}</h2>
<ul>
    <li v-if=" type !== 'multi_boolean' "
      v-for="choice in props.page.questions[props.etape].choices " 
      :key="choice" >
        <label  ><input type="checkbox" v-model="reponseBoolean" :value="choice">{{ choice }}</label>
    </li>
    
    <MultiBoolean  v-if="type === 'multi_boolean'"  
    :questions="questions"
    :etape="etape"
    :type="type"
    v-model:choix1="reponseMultieboolean[0]"
    v-model:choix2="reponseMultieboolean[1]"
    />
</ul>
    <div class="button">
      <button class="button1" v-if="etape >= 1 " @click="retirerReponse">Précédent</button>
      <button class="button2" @click="validerReponse">Suivant</button>

    </div>

</div>


</template>


<script setup>
import MultiBoolean from './multiBoolean.vue'
import {ref} from 'vue'

const reponseMultieboolean = ref([])
const reponseBoolean = ref([])

const props =defineProps({
    page : Object,
    etape : Number,
    type : String,
    suivant : Number,
    retirerReponse : Function
})



const emit = defineEmits(['suivant'])

const validerReponse = () => {
  let reponse

  if (props.type === 'multi_boolean') {
    reponse = reponseMultieboolean.value
  } 
  else {
    reponse = reponseBoolean.value
  }

  emit('suivant', reponse)
}



const questions = props.page.questions


</script>


<style scoped>

.button{
  display: flex;
}

.button2{
  margin-left: auto;
  display: block;
  opacity: 0.8;
  transition: .8s;

} 

.button1{
  margin-right: auto;
  display: block;
  opacity: 0.8;
  transition: .8s;

} 




.button2:active , .button1:active{
  transform: scale(0.5);
}
</style>