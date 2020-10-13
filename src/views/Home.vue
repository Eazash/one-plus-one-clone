<template>
  <div class="home">
    <div class="container">
      <div class="card question">
        <p>
          <span>1</span><span>+</span>
          <span>1</span>
          <span>+</span><span>2</span>
        </p>
      </div>
      <div class="card answers">
        <p class="answer">1</p>
        <p class="answer">2</p>
        <p class="answer">3</p>
        <p class="answer">4</p>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Home',
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
