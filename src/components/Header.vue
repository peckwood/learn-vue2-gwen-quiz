<template>
  <div>
    <b-nav>
      <b-nav-item disabled>Fancy Quiz App</b-nav-item>
      <b-nav-item disabled>Correct answers: {{correctAnswerCount}}</b-nav-item>
      <b-nav-item disabled>Submitted answers: {{totalAnswerCount}}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
    props: {
        correctAnswerCount: Number,
        totalAnswerCount: Number
    }
}
</script>
