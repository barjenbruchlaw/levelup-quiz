<script>
  import Quiz from "./Quiz.svelte"
</script>

<main>
  <h1>Hello world!</h1>
  <h4>I'm an h4</h4>
  <Quiz quizName="New Quiz" />
</main>

<style>
  :global(h4) {
    color: green;
  }
</style>
