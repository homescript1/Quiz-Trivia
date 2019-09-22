<template>
  <div id="app">
  
    <Header 
       :numCorrect = "numCorrect"
       :numTotal = "numTotal"
      />
      <b-container class="bv-example-row">
        <b-row>
          <b-col sm="6" offset="3"> <Question
            v-if="questions.length"
              :currentQuestion = "questions[index]"
              :currentNext = "next"
              :currentPrev = "prev"
              :increment = "increment"
              
          /></b-col>
        </b-row>
      </b-container>
     
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Question from './components/Question.vue'

export default {
  name: 'app',
  components: {
    Header,
    Question
  },
  data() {
    return {
      questions : [],
      index : 0,
      numCorrect :0,
      numTotal :0
    }
  },
  methods : {
    next : function (){
      if (this.index < 9)
          this.index ++
      else
        alert('This is the end')
    },
    prev : function () {
      if (this.index > 0)
          this.index --
      else
        alert('This is the start')
    },increment(isCorrect){
        if (isCorrect){
          this.numCorrect++
        }
        this.numTotal++
    }
  },
  mounted : function(){
    fetch('https://opentdb.com/api.php?amount=10',{
        method : 'get'
    }).then((response)=>{
      return response.json()
    }).then((jsonData) => {
        this.questions = jsonData.results
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
