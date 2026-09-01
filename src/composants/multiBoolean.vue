<template>
<h1>{{props.questions.question}}</h1>
<h3>{{ titre1 }}</h3>
<li v-for="choice1 in choices1" :key="choice1">
    <label ><input  type="radio" v-model="model1" :value="choice1">{{ choice1 }}</label>
</li>
<h3>{{ titre2 }}</h3>
<li v-for="choice2 in choices2" :key="choice2">
    <label ><input  type="radio" v-model="model2" :value="choice2">{{ choice2 }}</label>
</li>
{{ reponse }}
</template>




<script setup>
import {ref} from 'vue'


const props = defineProps({
    etape : Number,
    questions : Array,
    type : String
})

const model1 = defineModel('choix1')
const model2 = defineModel('choix2')

const titre1 = props.questions[props.etape].statements[0].text
const titre2 = props.questions[props.etape].statements[1].text
const choices1 = props.questions[props.etape].statements[0].choices
const choices2 = props.questions[props.etape].statements[1].choices


</script>

