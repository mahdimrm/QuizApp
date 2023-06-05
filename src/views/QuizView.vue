<script setup>
import QuestionHeader from "../components/QuizHeader.vue"
import Question from "../components/Question.vue"
import {useRoute} from "vue-router"
import quizes from "../data/3.1 data.json"
import Result from "../components/Result.vue"
import { ref ,computed} from "vue"
const route = useRoute()
const quizId = parseInt(route.params.id)

const quiz = quizes.find(q=> q.id==quizId);
const currentQuestionIndex = ref(0)
// const questionStatus = ref(`${currentQuestionIndex.value}/${quiz.questions.length}`)

// watch(() =>currentQuestionIndex.value, () =>{
//     questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`
// })

const questionStatus = computed(()=>`${currentQuestionIndex.value}/${quiz.questions.length}`)
const barPercentage = computed(() => `${currentQuestionIndex.value/quiz.questions.length*100}%`)
const numberOfCorrectAnswers = ref(0)
const showResults = ref(false)
const onOptionSelected = (isCorrect)=>{
    if(isCorrect)
    {
        numberOfCorrectAnswers.value++
    }

    if(quiz.questions.length - 1 === currentQuestionIndex.value)
    {
        showResults.value=true
    }
    currentQuestionIndex.value++
}
</script>   

<template>
    <QuestionHeader :questionStatus="questionStatus" :barPercentage="barPercentage"/>  
<div>
    <Question v-if="!showResults"  :question="quiz.questions[currentQuestionIndex]"  @selectOption="onOptionSelected"/> 
    <Result 
    v-else
    :quizQuestionLength="quiz.questions.length"
    :numberOfCorrectAnswers="numberOfCorrectAnswers" />
</div>
</template>
