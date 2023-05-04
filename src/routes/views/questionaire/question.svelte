<script lang="ts">
	export let index: number;
	export let question: string;
	$: question_parts = question.split("\n");
	export let options: string[];
	export let correct: number[];
	export let step: number;

	export let message_animation_delay: number;
	export let message_animation_length: number;

	// [option,original index]
	let sorted_options: [string, number][] = [];
	$: if (options != null) {
		sorted_options = [...options].map((option, i) => [option, i]);
		sorted_options.sort(() => Math.random() - 0.5);
		sorted_options = sorted_options;
	}

	import { slide } from "svelte/transition";

	function typewriter(node, { speed = 1, delay = 0 }) {
		const valid = node.childNodes.length === 1 && node.childNodes[0].nodeType === Node.TEXT_NODE;

		if (!valid) {
			throw new Error(`This transition only works on elements with a single text node child`);
		}

		const text = node.textContent;
		const duration = text.length / (speed * 0.01);

		return {
			delay,
			duration,
			tick: (t) => {
				const parts = text.split(" ");
				const i = Math.trunc(parts.length * t);
				node.textContent = parts.slice(0, i).join(" ");
			}
		};
	}
</script>

<div class="flex-1 flex gap-2 flex-col w-full">
	{#each question_parts as questionPart, part_index}
		<div class="message-container">
			<div class="profile-picture ai" class:hide={question_parts.length > 1 && part_index === 0} />
			<!--
				in:slide={{
					delay:
						message_animation_delay +
						(message_animation_delay + message_animation_length) * part_index,
					duration: message_animation_length
				}}
			-->
			<div
				class="w-full text-left message"
				in:typewriter={{
					speed: 5,
					delay:
						message_animation_delay +
						(message_animation_delay + message_animation_length) * part_index
				}}
			>
				{questionPart}
			</div>
		</div>
	{/each}

	{#if step > index}
		<div class="message-container">
			<div class="flex gap-1 justify-end w-full message own" in:slide>
				<!-- todo: change to selected answer later -->
				<p>{options[correct[0]]}</p>
			</div>
			<div class="profile-picture user" />
		</div>
	{/if}
</div>

<style>
	.message {
		box-sizing: border-box;
		padding: 0.25rem 1rem;
		margin: 0 0.5rem;
		background: #fff;
		border-radius: 1.125rem 1.125rem 1.125rem 0;
		min-height: 2.25rem;
		width: fit-content;
		max-width: 66%;
		font-size: medium;

		box-shadow: 0 0 2rem rgba(black, 0.075), 0rem 1rem 1rem -1rem rgba(black, 0.1);
	}
	.message:empty {
		display: none;
	}

	.own {
		margin-left: auto;
		border-radius: 1.125rem 1.125rem 0 1.125rem;
		background: #333;
		color: white;
	}

	.message-container {
		display: flex;
		align-items: center;
		max-width: 100%;
	}

	.profile-picture {
		border-radius: 50%;
		height: 25px;
		width: 25px;
		background-size: contain;
	}
	.profile-picture.hide {
		opacity: 0;
	}

	.ai {
		background-image: url("/sitra.png");
	}

	.user {
		background-image: url("https://i.pinimg.com/474x/76/4d/59/764d59d32f61f0f91dec8c442ab052c5.jpg");
	}
</style>
