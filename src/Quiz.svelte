<script>
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
        quiz = getQuiz();
    }

    function addToScore() {
        score = score + 1;
    }

</script>

<div>

    <button on:click={resetQuiz}>Start new Quiz</button>

    <h3>My Score: {score}</h3>
    <h4>Question #{activeQuestion+1}</h4>

    {#await quiz}
        Loading...
    {:then data}
        {#each data.results as question, index}
            {#if index === activeQuestion}
                <Question addToScore={addToScore} nextQuestion={nextQuestion} question={question} />
            {/if}
        {/each}
    {/await}
    
</div>