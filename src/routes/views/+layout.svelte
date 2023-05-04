<div class="flex flex-col mx-auto frame iphone-x">
	<div class="speaker" />
	<slot />
</div>

<style>
	:global(.iphone-x > *) {
		max-width: 100%;
	}

	@media only screen and (min-width: 600px) {
		.iphone-x {
			position: relative;
			margin: 40px auto;
			width: calc(100vh * 9 / 16);
			height: calc(100vh - calc(40px * 2));
			height: calc(100svh - calc(40px * 2));
			border-radius: 2.5rem;
			box-shadow: 0px 0px 0px 11px #1f1f1f, 0px 0px 0px 13px #191919, 0px 0px 0px 20px #111;
			box-sizing: content-box;
			max-height: 100%;
		}

		.iphone-x:before,
		.iphone-x:after {
			content: "";
			position: absolute;
			left: 50%;
			transform: translateX(-50%);
		}

		.iphone-x:before {
			top: 0px;
			width: 56%;
			height: 30px;
			background-color: #1f1f1f;
			border-radius: 0px 0px 40px 40px;
		}

		.speaker {
			position: absolute;
			display: block;
			color: transparent;

			top: 0px;
			left: 50%;
			transform: translate(-50%, 6px);
			height: 8px;
			width: 15%;
			background-color: #101010;
			border-radius: 8px;
			box-shadow: inset 0px -3px 3px 0px rgba(256, 256, 256, 0.2);
		}
	}

	.frame {
		/* force aspect-ratio of 9 / 16 */
		width: calc(100vh * 8 / 16);
		max-width: 100vw;
		/* border: 1em solid black; */
		/* border-radius: 1em; */
		/* base font size on view height */
		font-size: calc(1vh * 0.5);
	}
</style>
