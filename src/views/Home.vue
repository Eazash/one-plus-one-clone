<template>
  <div class="home">
    <div class="container">
      <div class="card question">
        <p v-text="questionString"></p>
        <progress-bar :max="timeToAnswer" :current="remainingTime">
        </progress-bar>
      </div>
      <div class="card answers" v-if="!fail">
        <answer-block
          v-for="answer in answers"
          class="answer"
          :key="answer"
          :number="answer"
          @chosen="chosen"
        />
      </div>
      <div class="card fail" v-else>
        <div>
          <h2>Game Over</h2>
        </div>
        <div>
          <button @click="newGame">New Game</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AnswerBlock from '@/components/AnswerBlock.vue';
import ProgressBar from '@/components/ProgressBar.vue'
import { addSeconds, differenceInMilliseconds } from 'date-fns'

export default {
  components: { AnswerBlock, ProgressBar },
  data() {
    return {
      questionString: '',
      answer: '',
      base: 1,
      level: 1,
      answers: [],
      fail: false,
      round: 1,
      now: new Date(),
      interval: 0,
      deadline: addSeconds(new Date(), 30),
      timeToAnswer: 30,
      positives: [],
      negatives: []
    };
  },
  name: 'Home',
  created() {
    this.newQuestion();
  },
  computed: {
    remainingTime: function () {
      return (differenceInMilliseconds(this.deadline, this.now) / 1000)
    },
    sumOfPositives: function () {
      return this.positives.reduce(this.add);
    },
    sumOfNegatives: function () {
      return this.negatives.reduce(this.add);
    },
  },
  methods: {
    newGame() {
      this.fail = false;
      this.round = 1;
      this.newQuestion();
    },
    gameOver() {
      clearInterval(this.interval);
      this.fail = true;
      this.deadline = this.now;
    },
    newQuestion() {
      this.generateQuestion();
      this.createQuestionString();
      this.createAllAnswers();
      this.round++;
      this.now = new Date();
      this.interval = setInterval(() => this.now = new Date(), 200);
      this.deadline = addSeconds(this.now, this.timeToAnswer);
    },
    generateQuestion() {
      this.negatives = [];
      this.positives = [];
      do {
        this.positives.push(this.base + this.RANDOM(2));
      } while (this.positives.length < 3)
      do {
        this.negatives.push(-1 * (this.base + this.RANDOM()))
        if (this.sumOfNegatives > this.sumOfPositives) {
          this.negatives.pop();
          break;
        }
      } while (this.negatives.length < 1)
      this.answer = this.sumOfPositives + this.sumOfNegatives;
    },
    createQuestionString() {
      let numbers = [...this.positives, ...this.negatives];
      numbers = this.shuffle(numbers);
      const numbersString = numbers.join('+');
      this.questionString = numbersString.replace("+-", "-");
    },
    add: (total, value) => {
      return total + value
    },
    shuffle(array) {
      for (let i = array.length - 1; i >= 0; i--) {
        const j = this.RANDOM(i);
        [array[i], array[j]] = [array[j], array[i]]
      }
      return array;
    },
    RANDOM(max = 1, min = 0) {
      return min + Math.floor(Math.random() * max);
    },
    createAllAnswers() {
      const actual = parseInt(this.answer, 10);
      let deviation = 1;
      let wrongAnswer = 0;
      this.answers = [];
      while (this.answers.length < 3) {
        const delta = this.RANDOM(deviation);
        if (this.RANDOM(deviation) % 2 === 0) {
          wrongAnswer = actual + delta;
        } else {
          wrongAnswer = actual - delta;
        }
        if (this.answers.includes(wrongAnswer) || wrongAnswer === actual || wrongAnswer === 0) {
          deviation += 1;
          continue;
        } else {
          this.answers.push(wrongAnswer);
        }
      }
      this.answers.push(this.answer);
      this.answers = this.shuffle(this.answers);
    },
    chosen(number) {
      if (number === this.answer) {
        this.newQuestion();
      } else {
        this.gameOver();
      };
    }
  },
  watch: {
    remainingTime: function (value) {
      if (value <= 0 && !this.fail) {
        this.gameOver();
      };
    }
  },
};
</script>
<style lang="scss">
.container {
  margin: 0 auto;
  width: 80%;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  div {
    margin: 10px;
  }
}
.card {
  width: 100%;
  max-width: 500px;
  margin: auto;
  border: 1px solid grey;
  border-radius: 3px;
  display: flex;
  flex-direction: column;
  align-items: center;
  p {
    min-width: 50%;
    margin: auto;
  }
}
.question {
  box-shadow: 0 1px 5px grey;
  height: 20vh;
  font-weight: 700;
  font-size: 2rem;
  letter-spacing: 1rem;
}
.answers {
  height: 40vh;
  font-weight: 600;
  font-size: 1rem;
  .answer {
    width: 80%;
    margin: auto 0;
    border: 1px solid grey;
    padding: 10px;
    border-radius: 25px;
    box-shadow: 0 1px 5px grey;
    transition: all 0.3s;
    &:hover {
      box-shadow: 0 4px 5px grey;
    }
  }
}
.fail {
  border: 2px solid red;
  color: red;
  text-align: center;
  display: block;
  padding: 1rem;
  font-weight: 600;
  h2 {
    padding-bottom: 1rem;
    border-bottom: 4px solid rgb(252, 126, 126);
  }
  button {
    background: #e4e4e4;
    outline: none;
    border: none;
    width: 220px;
    border-radius: 5px;
    padding: 10px;
    font-size: 1.25rem;
    margin: 10px auto;
    color: #42b983;
    font-weight: 700;
    transition: all 0.3s;
    &:hover {
      box-shadow: 1px 3px 6px grey;
    }
  }
}
</style>
