<script>
  import { dataset_dev } from 'svelte/internal'
  import { get } from 'svelte/store'
  import Question from './Question.svelte'

  let quiz = getQuiz()
  let activeQuestion = 0
  async function getQuiz() {
    const res = await fetch(
      'https://opentdb.com/api.php?amount=10&category=24&type=multiple'
    )
    const quiz = await res.json()
    return quiz
  }
  function handleClick() {
    quiz = getQuiz()
  }

  function nextQuestion() {
    activeQuestion += 1
  }
</script>

<div>
  <button on:click={handleClick}>Start New Quiz</button>

  <h3>My Score: 0</h3>
  <h4>Question #{activeQuestion + 1}</h4>

  {#await quiz}
    Loading ...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <Question {nextQuestion} {question} />
      {/if}
    {/each}
  {/await}
</div>

<style></style>
