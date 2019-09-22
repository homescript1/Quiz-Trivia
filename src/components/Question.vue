<template>
   <div class="question-box-container">
       <div>
        <b-jumbotron>
           

            <template v-slot:lead>
               {{currentQuestion.question}}
            </template>

            <hr class="my-4">
            <b-list-group>

            <b-list-group-item v-for="(answer,index) in answers" :key="index" @click="selectAnswers(index)" :class="[
                !answered && (selectIndex === index ) ? 'selected' : 
                answered && (correctIndex === index) ? 'correct' :
                answered && ((selectIndex===index) && (correctIndex !== index) ) ? 'incorrect' : ''
             ]">
                {{answer}}
            </b-list-group-item>
            </b-list-group>
            
             <b-button variant="danger" @click="currentPrev">Prev</b-button>
            <b-button variant="success" @click="currentNext" >Next</b-button>
             <br/>
              <b-button variant="primary" @click="submitAnswers" :disabled=" selectIndex === null || answered " >Submit</b-button>
        </b-jumbotron>
        </div>
    </div>
</template>

<script>
import _ from 'lodash'

export default {
    props : {
        currentQuestion  : Object,
        currentPrev : Function,
        currentNext : Function,
        increment : Function
    },data (){
        return {
            selectIndex : null,
            shuffleAnswers : [],
            correctIndex : null,
            answered : false
        }
    },
     watch :{
        currentQuestion(){
            this.selectIndex =null
            this.shuffleAnswer()
            this.answered = false
        }
    },
    computed : {
        answers : function() {
           let answers = [...this.currentQuestion.incorrect_answers]
           answers.push(this.currentQuestion.correct_answer)
           return answers
        }
    },
    methods : {
         selectAnswers : function (index) {
            this.selectIndex = index
        },
        shuffleAnswer(){
            let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
            this.shuffleAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffleAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswers(){
            let isCorrect = false

            if (this.selectIndex == this.correctIndex){
                isCorrect = true 
            }
           
            this.increment(isCorrect)
            this.answered = true
        }
    }
   
}
</script>

<style scoped>
 .list-group{
     margin-bottom: 15px;
 }

 btn { 
     margin : 0 5px;
 }

 .list-group-item:hover {
     background : #EEE;
     cursor: pointer;
 }

 .selected {
     background-color: #66ccff;
 }

 .correct {
     background-color: green;
     color : white;
 }

 .incorrect {
     background-color: red;
     color : black;
 }


</style>