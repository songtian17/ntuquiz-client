<script setup>
import { ref } from 'vue';

const quizzes = ref([])

fetch('https://8z5ooth70m.execute-api.ap-southeast-1.amazonaws.com/default/quiz').then((res) => {
  return res.json();
}).then((data) => {
  quizzes.value = data;
})

function getQuizQuestions(index) {
  const quiz = quizzes.value[index];
  fetch(`https://8z5ooth70m.execute-api.ap-southeast-1.amazonaws.com/default/quiz?course=${encodeURIComponent(quiz.course)}&quiz_name=${encodeURIComponent(quiz.quiz_name)}`).then((res) => {
    return res.json();
  }).then((data) => {
    console.log(data)
    quizzes.value[index].questions = data.questions;
  })
}

</script>

<template>
    <li v-for="(quiz, index) in quizzes">
      <div>
        <a>{{ quiz.course }}</a>
        <li>
          <a @click="getQuizQuestions(index)">{{ quiz.quiz_name }}</a>
          <li v-for="question in quiz.questions">
            <b>Q: {{ question.question }}</b>
            <li v-for="answer in question.correct_answers" class="correct_answer">
              <span>{{ answer }}</span>
            </li>
            <li v-for="answer in question.incorrect_answers" class="incorrect_answer">
              <span >{{ answer }}</span>
            </li>
          </li>
        </li>
      </div>
    </li>
</template>

<style scoped>
li {
  list-style: none;
  padding: none;
  margin-left: 1em;
}


a {
  color: #0069c2;
  text-decoration: underline;
  cursor: pointer;
}

a:hover {
  text-decoration: none;
}

a:active {
  background-color: #0069c2;
  color: white;
}

.correct_answer {
  list-style-type: disc;
  span {
    background-color: lightgreen;
  }
}

.incorrect_answer {
  list-style-type: circle;
  span {
    background-color: lightcoral;
  }
}
</style>
