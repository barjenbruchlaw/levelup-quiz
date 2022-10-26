<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition"
  import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte"
  import Question from "./Question.svelte"
  import Modal from "./Modal.svelte"
  let quiz = getQuiz()
  let activeQuestion = 0
  let score = 0
  let oneMoreCorrect = false
  let isModalOpen = false

  onMount(() => {
    console.log("I mounted")
  })

  beforeUpdate(() => {
    console.log("Before update")
  })

  afterUpdate(() => {
    console.log("After update")
  })

  async function getQuiz() {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=24&type=multiple"
    )
    const quiz = await res.json()
    return quiz
  }

  function nextQuestion() {
    activeQuestion += 1
  }

  function resetQuiz() {
    isModalOpen = false
    score = 0
    activeQuestion = 0
    oneMoreCorrect = false
    quiz = getQuiz()
  }

  function addToScore() {
    score += 1
  }

  //Reactive statement
  // $: if (score > 6) {
  //   oneMoreCorrect = true
  // }

  $: if (score > 2) {
    isModalOpen = true
  }

  // $: if (score < 8 && activeQuestion === 10) {
  //   alert("Sorry, try again.")
  //   resetQuiz()
  // }

  $: questionNumber = activeQuestion + 1
</script>

<div>
  <button on:click={resetQuiz}>Start New Quiz</button>

  <h3>My Score: {score}</h3>
  <h4>Question #{questionNumber}</h4>
  {#if oneMoreCorrect}
    <h4 class="oneMore">One more to go!</h4>
  {/if}
  {#await quiz}
    Loading ...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <div in:slide={{ x: 100 }} out:fly={{ x: -200 }} class="fade-wrapper">
          <Question {addToScore} {nextQuestion} {question} />
        </div>
      {/if}
    {/each}
  {/await}
</div>

{#if isModalOpen}
  <Modal on:close={resetQuiz}>
    <h2>You won!</h2>
    <p>Congratulations</p>
    <button on:click={resetQuiz}>Start Over</button>
  </Modal>
{/if}

<style>
  .fade-wrapper {
    position: absolute;
  }

  .oneMore {
    background-color: green;
    color: white;
  }
</style>
