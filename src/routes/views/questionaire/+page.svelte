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
		actor_pic: string | undefined;
	};
	let question: Question | null;
	$: question = questions[step];
	$: question_options = question?.options;
	$: question_correct_options = question?.correct;

	let message_container: HTMLElement;
</script>

<div class="all-container flex flex-col gap-4 mt-4 mb-2 flex-1">
	<div class="flex flex-col header">
		<h2>Questionnaire</h2>
	</div>
	<div class="messages-container flex-1 flex gap-2 flex-col" bind:this={message_container}>
		<div class="messages-parent flex-1">
			<div class="flex-1 flex flex-col-reverse gap-3">
				{#each questions_reversed as question, i}
					{@const corrected_i = questions.length - i - 1}
					{#if step >= corrected_i}
						<Question
							index={corrected_i}
							{...question}
							{message_animation_delay}
							{message_animation_length}
							bind:step
							bind:container={message_container}
						/>
					{/if}
				{/each}
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
	<div id="answer-options" class="w-full grid grid-cols-1 grid-rows-1 gap-2 card p-2">
		{#key question_options}
			{#each question_options || [] as option, i}
				{@const is_valid = question_correct_options?.includes(i)}
				<button
					class="btn variant-ghost-primary"
					disabled={!is_valid}
					on:click={() => {
						if (is_valid) {
							answers = [...answers, i];

							// scroll to bottom
							message_container.scroll({ top: message_container.scrollHeight, behavior: "smooth" });
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
	<div class="flex gap-2 items-center">
		<progress value={$progress} />
		<p class="whitespace-pre">{step + 1} / {questions.length}</p>
	</div>
</div>

<style>
	.header {
		padding: 15px;
		gap: 0.25rem;
	}

	.all-container {
		margin: 30px 20px;
		max-height: 90%;
	}

	.messages-container {
		overflow-y: scroll;
	}
</style>
