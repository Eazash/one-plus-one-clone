<template>
  <div class="home">
    <div class="container">
      <div class="card question">
        <p v-text="question"></p>
      </div>
      <div class="card answers">
        <p class="answer" v-text="answer"></p>
        <p class="answer">2</p>
        <p class="answer">3</p>
        <p class="answer">4</p>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      question: '',
      answer: '',
      base: 1,
      level: 1,
      SEED: 1,
    };
  },
  name: 'Home',
  mounted() {
    this.generateQuestion();
    this.calculateAnswer();
  },
  methods: {
    generateQuestion() {
      const numbers = [];
      const operations = [];
      for (let counter = 0; counter < 3; counter += 1) {
        numbers.push(this.base + this.RANDOM());
      }
      for (let operationsCounter = 0; operationsCounter < 2; operationsCounter += 1) {
        operations.push((this.RANDOM()) ? '+' : '-');
      }
      this.question = this.concat(numbers, operations);
    },
    concat(numbers, operations) {
      let concatenatedString = '';
      numbers.forEach((number, index) => {
        concatenatedString = concatenatedString.concat(number);
        if (operations[index] !== undefined) {
          concatenatedString = concatenatedString.concat(operations[index]);
        }
      });
      console.log(concatenatedString, numbers, operations);
      return concatenatedString;
    },
    RANDOM() {
      return Math.round(Math.random() * this.SEED);
    },
    calculateAnswer() {
      const operationTest = /(\d|-\d)[+-](\d|-\d)/;
      let questionString = this.question;
      let operation = operationTest.exec(questionString);
      let result;
      while (operation !== null) {
        console.log(operation);
        const [opString, number1, number2] = operation;
        if (opString[1].includes('+')) {
          result = parseInt(number1, 10) + parseInt(number2, 10);
        } else {
          result = parseInt(number1, 10) - parseInt(number2, 0);
        }
        questionString = questionString.replace(opString, result);
        console.log(result, questionString);
        operation = operationTest.exec(questionString);
      }
      this.answer = questionString;
    },
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
  flex-direction: column;
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
</style>
