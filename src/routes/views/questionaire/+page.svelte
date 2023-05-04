<script lang="ts">
	import questions from "./questions.json";
	$: questions_reversed = [...questions].reverse() as Question[];
	let message_animation_delay = 1000;
	let message_animation_length = 500;

	import { scale } from "svelte/transition";
	import Question from "./question.svelte";
	import { browser } from "$app/environment";
	let answers: number[] = [];
	$: step = answers.length;

	import { tweened } from "svelte/motion";
	import { cubicOut } from "svelte/easing";
	const progress = tweened(0, {
		duration: 400,
		easing: cubicOut
	});
	$: progress.set(step / questions.length);

	type Question = {
		question: string;
		options: string[];
		correct: number[];
	};
	let question: Question | null;
	$: if (browser) question = questions[step];
	$: question_options = question?.options;
	$: question_correct_options = question?.correct;
	let sorted_options: any[] = [];
	$: if (question_options != null) {
		sorted_options = [...question_options].map((option, i) => [option, i]);
		//sorted_options.sort(() => Math.random() - 0.5);
		//sorted_options = sorted_options;
	} else {
		sorted_options = [];
	}
</script>

<div class="all-container flex flex-col gap-4 mt-4 mb-2 flex-1">
	<div class="flex flex-col header">
		<h1>Questionnaire</h1>
		<p class="text-xs mt-px card p-4 variant-ghost">
			This is a demo, so to keep things simple, the answers are prefilled.
		</p>
		<progress value={$progress} />
	</div>
	<div class="messages-container flex-1 flex gap-2 flex-col">
		<div class="messages-parent flex-1">
			<div class="flex-1 flex flex-col-reverse">
				{#if browser}
					{#each questions_reversed as question, i}
						{@const corrected_i = questions.length - i - 1}
						{#if step >= corrected_i}
							<Question
								index={corrected_i}
								{...question}
								{message_animation_delay}
								{message_animation_length}
								bind:step
							/>
						{/if}
					{/each}
				{/if}
			</div>
		</div>

		<!-- <div id="answer-input" class="w-full flex gap-2">
			<input class="input" type="text" placeholder="type your answer to add context" disabled />
			<button
				class="btn variant-ghost-primary"
				on:click={() => {
					step += 1;
				}}>advance</button
			>
		</div> -->
	</div>
	<div id="answer-options" class="w-full grid grid-cols-2 grid-rows-3 gap-2">
		{#key sorted_options}
			{#each sorted_options as [option, original_i], i}
				{@const is_valid = question_correct_options.includes(original_i)}
				<button
					class="btn variant-ghost-primary"
					disabled={!is_valid}
					on:click={() => {
						if (is_valid) {
							answers = [...answers, original_i];
						}
					}}
					in:scale={{
						delay: message_animation_delay + message_animation_length + i * 200,
						duration: 100
					}}
				>
					{option}
				</button>
			{/each}
		{/key}
	</div>
</div>

<style>
	.header {
		padding: 15px;
		gap: 0.25rem;
	}

	.all-container {
		margin: 30px 20px;
		max-height: 100%;
	}

	.messages-container {
		overflow-y: scroll;
		max-height: 50%;
	}
</style>
