<script>
    import { fade, blur, fly, slide, scale } from "svelte/transition";
    import Question from "./Question.svelte";

    let quiz = getQuiz();
    let activeQuestion = 0;
    let score = 0;

    async function getQuiz() {
        const res = await fetch("https://opentdb.com/api.php?amount=10&category=12&type=multiple")
        const quiz = await res.json();
        console.log(quiz);
        return quiz;
    }

    function nextQuestion() {
        activeQuestion = activeQuestion + 1;
    }

    function resetQuiz() {
        score = 0;
        activeQuestion = 0;
        quiz = getQuiz();
    }

    function addToScore() {
        score = score + 1;
    }

    $: if (score > 6) {
        alert("You won!");
        resetQuiz();
    }

    $: questionNumber = activeQuestion + 1;

</script>

<style>
    .fade-wrapper {
        position: absolute;
    }
</style>

<div>

    <button on:click={resetQuiz}>Start new Quiz</button>

    <h3>My Score: {score}</h3>
    <h4>Question #{questionNumber}</h4>

    {#await quiz}
        Loading...
    {:then data}
        {#each data.results as question, index}
            {#if index === activeQuestion}
                <div in:fly={{x: 100}} out:fly={{x: -200}} class="fade-wrapper">
                    <Question addToScore={addToScore} nextQuestion={nextQuestion} question={question} />
                </div>
            {/if}
        {/each}
    {/await}
    
</div>