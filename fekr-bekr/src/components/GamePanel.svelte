<script lang="ts">
	import { onMount } from "svelte";
	import GuessLine from "./GuessLine.svelte";

	export let gameStatus: { start?: boolean; end?: boolean; winner?: boolean; losser?: boolean } = {};
	export let gameConfig: { randomCount?: number; guessCount?: number } = {};
	let randomNumbers: number[] = [];
	let inputNumbers: HTMLInputElement[][] = [];
	let displaies: boolean[] = [];
	let rownumber = 0;
	let winner: boolean = false;

	function randomIntFromInterval(min, max) {
		return Math.floor(Math.random() * (max - min + 1) + min);
	}

	function btnCheckedClick() {
		if (winner) {
			gameStatus.end = true;
			//alert("You are Winer");
			return;
		}

		if (rownumber < gameConfig.guessCount) {
			displaies[++rownumber] = true;
			if (rownumber == gameConfig.guessCount) {
				gameStatus.end = true;
				//alert("You are Loser");
			}
			return;
		}
	}
	
	function btnResetClick() {
		winner = null;
		gameStatus.end = false;
		gameStatus.start = false;
	}
	onMount(() => {
		for (let i: number = 0; i < gameConfig.randomCount; ) {
			let rNo: number = randomIntFromInterval(0, 9);
			let len: number = randomNumbers.filter((r) => r == rNo).length;
			if (len == 0) {
				randomNumbers = [...randomNumbers, rNo];
				i++;
			}
		}

		for (let i: number = 0; i < gameConfig.guessCount; i++) {
			let inNumbers: HTMLInputElement[] = [];

			for (let j: number = 0; j < gameConfig.randomCount; j++) {
				displaies = [...displaies, false];
				inNumbers = [...inNumbers, <HTMLInputElement>document.createElement("input")];
			}

			inputNumbers = [...inputNumbers, inNumbers];
		}
		displaies[rownumber] = true;
	});
</script>

<div class="card w-75 m-auto mt-3 rounded shadow">
	<div class="card-header d-flex justify-content-center">
		<div class="row">
			{#each randomNumbers as number}
				{#if gameStatus.end == false}
					<div class="col">
						<input
							value="*"
							class="form-control form-control-lg shadow my-3"
							style="width: 50px;text-align: center;"
							type="text"
							aria-label=".form-control-lg example"
						/>
					</div>
				{:else}
					<div class="col">
						<input
							value={number}
							class="form-control form-control-lg shadow my-3"
							style="width: 50px;text-align: center;"
							type="text"
							readonly
							aria-label=".form-control-lg example"
						/>
					</div>
				{/if}
			{/each}
		</div>
	</div>
	{#each inputNumbers as inputs, i}
		<GuessLine bind:winner display={displaies[i]} rowNumber={i + 1} bind:randomNumbers bind:inputNumbers={inputs} {btnCheckedClick} />
	{/each}
	<div class="col">
		{#if gameStatus.end && winner}
			<div class="alert alert-success m-3 shadow" role="alert">
				<h4 class="alert-heading">You Are A Winner <i class="bi bi-emoji-smile" /></h4>

				<hr />
				<button on:click={btnResetClick} type="button" class="btn btn-success">Restart</button>
			</div>
		{:else if gameStatus.end && !winner}
			<div class="alert alert-danger m-3 shadow" role="alert">
				<h4 class="alert-heading">You Are A Looser <i class="bi bi-emoji-frown" /></h4>

				<hr />
				<button on:click={btnResetClick} type="button" class="btn btn-danger">Restart</button>
			</div>
		{/if}
	</div>
</div>
