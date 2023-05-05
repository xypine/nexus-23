<script lang="ts">
	import { browser } from "$app/environment";
	import { ConicGradient, ProgressRadial, type ConicStop, Avatar } from "@skeletonlabs/skeleton";
	import { onMount } from "svelte";
	import { cubicOut } from "svelte/easing";
	import { fly, scale, slide } from "svelte/transition";

	const conicStops: ConicStop[] = [
		{ label: "One", color: "#bcd3b3", start: 0, end: 10 },
		{ label: "Two", color: "#488740", start: 10, end: 15 },
		{ label: "", color: "transparent", start: 15, end: 100 }
	];

	let score: number = 10;
	let ready = false;
	onMount(() => {
		setTimeout(() => {
			ready = true;
		}, 500);
	});
</script>

<div class="all-container flex flex-col gap-4 mt-4 mb-2 flex-1">
	<div class="flex flex-col header">
		<div class="flex justify-between">
			<h2>Results</h2>
			<a class="btn variant-ghost-secondary" href="/">
				<img src="/icons/home.svg" alt="home" />
			</a>
		</div>
	</div>
	{#if ready}
		<div
			class="card variant-ghost p-4 mx-4 flex-1 flex flex-col items-center gap-2"
			in:fly={{ delay: 500, y: 50, duration: 500, easing: cubicOut }}
		>
			<div id="top" class="flex-1 w-full flex justify-center items-center">
				<ProgressRadial
					class="w-10/12"
					stroke={100}
					meter="stroke-primary-500"
					track="stroke-primary-500/30"
					value={score}>2600kg CO₂e</ProgressRadial
				>
			</div>
			<div id="bottom" class="flex-1 w-full text-center">
				<h3>An example for others</h3>
				<p>
					You excell at making great choices for the environment, yet you could still improve your
					habits.
				</p>
			</div>
			<div>
				<b class="text-base">Leaderboard</b>
				<div class="table-container max-w-full">
					<table class="table table-fixed">
						<tbody>
							<tr>
								<td>11.</td>
								<td>
									<Avatar src="https://i.pravatar.cc/?img=48" width="w-8" rounded="rounded-full" />
								</td>
								<td> Iida </td>
								<td>2000kg</td>
							</tr>
							<tr class="font-semibold">
								<td>12.</td>
								<td>
									<Avatar src="https://i.pravatar.cc/?img=47" width="w-8" rounded="rounded-full" />
								</td>
								<td>You</td>
								<td>2600kg</td>
							</tr>
							<tr>
								<td>13.</td>
								<td>
									<Avatar src="https://i.pravatar.cc/?img=42" width="w-8" rounded="rounded-full" />
								</td>
								<td>Petri</td>
								<td>3000kg</td>
							</tr>
						</tbody>
					</table>
				</div>
			</div>
			<a class="btn variant-filled-primary" href="/view/challenges">Help me improve my score</a>
		</div>
	{:else}
		<div class="flex-1 flex justify-center items-center">
			<ProgressRadial />
		</div>
	{/if}
</div>

<style>
	.header {
		padding: 15px;
		padding-bottom: 0;
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
