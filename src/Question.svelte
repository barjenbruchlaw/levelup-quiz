<script>
  export let question

  let answers = question.incorrect_answers
</script>

<h3>{question.question}</h3>

{#each answers as answer}
  <button>{answer}</button>
{/each}
