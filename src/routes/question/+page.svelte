<script>
	// @ts-nocheck

	import quizData from '$lib/questions.json';

	let answers = [];
	let question = null;
	let selectedAnswer = null;
	let correctAnswer = null;

	function pickQuestion() {
		const randomIndex = Math.floor(Math.random() * quizData.length);
		question = quizData[randomIndex].question;
		answers = quizData[randomIndex].options;
		correctAnswer = quizData[randomIndex].answer;
		selectedAnswer = null; // Reset selection when picking a new question
	}

	function checkAnswer(answer) {
		selectedAnswer = answer;
	}

    function reset() {
        answers = [];
        question = null;
        selectedAnswer = null;
        correctAnswer = null;
        pickQuestion();
    }

	pickQuestion();
</script>

<div class="flex items-center h-screen justify-center">
	<div class="text-center">
		<h1 class="text-4xl pb-8 text-red-300">JCC Food Festival Trivia</h1>
		<h2 class="text-2xl text-slate-100">Question: {question}</h2>

		<div class="pt-20 pb-20 justify-center grid grid-cols-2 place-items-center">
			{#each answers as answer}
				<button
					disabled={selectedAnswer != null}
					on:click={() => checkAnswer(answer)}
					class="butan self-center {selectedAnswer
						? answer === correctAnswer
							? 'correct'
							: 'incorrect'
						: ''}"
					>{answer}
				</button>
			{/each}
		</div>

		<div>
			{#if selectedAnswer != null && selectedAnswer === correctAnswer}
				<div role="alert" class="alert alert-success">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="h-6 w-6 shrink-0 stroke-current"
						fill="none"
						viewBox="0 0 24 24"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
						/>
					</svg>
					<span>You got it right!</span>
				</div>
			{:else if selectedAnswer != null && selectedAnswer !== correctAnswer}
                <div role="alert" class="alert alert-error">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 shrink-0 stroke-current" fill="none" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
                    </svg>
                    <span>Unlucky! You may have gotten it wrong, but try again!</span>
                </div>
			{/if}
            {#if selectedAnswer != null}
                <div>
                    <button on:click={reset} class="butan">
                        Play Again
                    </button>
                </div>
            {/if}
		</div>
	</div>
</div>

<style>
	.correct:disabled {
		background-color: green;
		color: white;
	}

	.incorrect:disabled {
		background-color: red;
		color: white;
	}
</style>
