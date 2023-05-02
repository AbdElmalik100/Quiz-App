<script setup>
import { ref, computed, watch, onMounted } from 'vue'
const questions = ref([
  {
    question: 'What is vue js?',
    answer: 0,
    options: [
      'A front end framework',
      'A library',
      'An ice cream maker'
    ],
    selected: null
  },
  {
    question: 'What is vuex js?',
    answer: 1,
    options: [
      'Vue with an x',
      'A state managment',
      'A cheese selection'
    ],
    selected: null
  },
  {
    question: 'What is vue router used for?',
    answer: 1,
    options: [
      'Walking in the space',
      'A routing library for vue JS',
      'A gumball',
    ],
    selected: null
  },
])
const quizCompleted = ref(false)
const currentQuestion = ref(0)

const duration = ref(10)
const Timer = ref(null)

const time = ref(30)

const score = computed(() => {
  let theScore = 0
  questions.value.map(quiz => {
    if (quiz.selected === quiz.answer) {
      theScore++
    }
  })
  return theScore
})

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

let setAnswer = (event) => {
  questions.value[currentQuestion.value].selected = parseInt(event.target.value)
  event.target.value = null
}
let nextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
  } else quizCompleted.value = true
}


let countDownTimer = () => {
  // let minutes, seconds
  let handler = setInterval(() => {
    time.value--
    console.log(time.value);
    if (time.value < 0) {
      clearInterval(handler)
      quizCompleted.value = true
    }
    // ---------------------------- Timer With Minutes And Seconds :) If You Forget How To Do A Timer Come Here And Take A Look ;)
    // minutes = parseInt(duration.value / 60)
    // seconds = parseInt(duration.value % 60)
    // minutes = minutes < 10 ? `0${minutes}` : minutes
    // seconds = seconds < 10 ? `0${seconds}` : seconds
    // Timer.value = `${minutes}:${seconds}`
    // duration.value--
    // if (duration.value < -1) {
    //   clearInterval(handler)
    //   quizCompleted.value = true
    // }
  }, 1000)
}
onMounted(() => {
  countDownTimer()
})


</script>

<template>
  <main class="app bg-dark min-vh-100 text-light  py-3">
    <h1 class="text-center">Quiz App</h1>
    <section class="quiz container" v-if="!quizCompleted">
      <div class="info d-flex align-items-center justify-content-between mb-3">
        <h3 class="question m-0">{{ getCurrentQuestion.question }}</h3>
        <div class="alt-info text-white-50">
          <span class="quiz-num me-4">Q{{ currentQuestion + 1 }} / Q{{ questions.length }}</span>
          <span class="score">Score {{ score }}/{{ questions.length }}</span>
        </div>
      </div>
      <div class="option">
        <label v-for="(option, index) in getCurrentQuestion.options" :key="index" :class="`${getCurrentQuestion.selected === index ? index === getCurrentQuestion.answer ? 'correct' : 'wrong' : ''}
        ${getCurrentQuestion.selected !== null && index !== getCurrentQuestion.selected ? 'disabled' : ''}`">
          <input type="radio" :name="getCurrentQuestion.index" :value="index" @change="setAnswer"
            v-model="getCurrentQuestion.selected">
          <span>{{ option }}</span>
        </label>
      </div>
      <div class="d-flex align-items-center justify-content-between mt-4">
        <button class="submit" @click="nextQuestion"
          v-text="`${currentQuestion === questions.length - 1 ? 'Finish' : getCurrentQuestion.selected !== null ? 'Next Question' : 'Select An Option'}`"
          :disabled="getCurrentQuestion.selected === null"></button>
        <span class="fs-2">{{ `00:${time < 10 ? `0${time}` : time}` }}</span>
      </div>
    </section>
    <section v-else class="text-center mt-5">
      <h2>You Have Finished The Quiz</h2>
      <p class="mt-4 text-white-50">Your Score is {{ score }}/{{ questions.length }}</p>
    </section>
  </main>
</template>

<style lang="scss">
* {
  box-sizing: border-box;
}

.quiz {
  padding: 20px;
  margin-top: 50px;
  max-width: 750px;
  width: 100%;
  border-radius: 10px;
  background-color: #4d4d4d;

  .option {
    display: flex;
    flex-direction: column;
    gap: 10px;

    label {
      background-color: #2b2b2b;
      font-size: 20px;
      font-weight: bold;
      width: 100%;
      padding: 20px;
      border-radius: 10px;
      transition: 0.2s;
      cursor: pointer;

      &:hover {
        background-color: #3d3d3d;
      }

      &.disabled {
        opacity: 0.3;
        pointer-events: none;
      }

      &.wrong {
        background-color: #ac2727;
      }

      &.correct {
        background-color: #57af5d;
      }
    }

    input {
      display: none;
    }
  }

  .submit {
    background-color: #62b666;
    color: white;
    border: none;
    outline: none;
    transition: 0.2s;
    text-transform: uppercase;
    font-weight: bold;
    border-radius: 10px;
    padding: 15px 30px;
    margin-top: 10px;
    text-align: left;

    &:hover {
      background-color: #4f9153;
    }

    &:disabled {
      opacity: 0.3;
      pointer-events: none;
    }
  }
}
</style>
