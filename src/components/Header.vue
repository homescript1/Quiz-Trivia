<template>
  <div>
    <b-nav>
      <b-nav-item disabled>
        <strong>QuestionBox</strong>
      </b-nav-item>
      <b-nav-item disabled>
        Correct: {{numCorrect}}    Total Questions : {{numTotal}}
      </b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
    props : [
        'numCorrect',
        'numTotal'
    ]
}
</script>
