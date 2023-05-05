<script>
	import { fade } from "svelte/transition";
</script>

<div class="flex-1 flex flex-col gap-3 justify-center items-center text-center">
	<h1>Demo complete</h1>
</div>

<style>
	@keyframes fadein {
		0% {
			opacity: 0;
		}
		66% {
			opacity: 0;
		}
		100% {
			opacity: 1;
		}
	}
	.fade {
		animation: 3s ease 0s normal forwards 1 fadein;
	}
</style>
