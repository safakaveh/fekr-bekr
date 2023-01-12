<script lang="ts">
	import { onMount } from "svelte";

	let count: number;
	export let rowNumber: number;
	export let winner: boolean = false;
	export let inputNumbers: HTMLInputElement[] = [];
	export let randomNumbers: number[] = [];
	export let display = false;

	let checked: boolean = false;

	function checkGuess(no: number, location: number): number {
		if (randomNumbers[location] == no) {
			return 1;
		}
		let found: number = randomNumbers.filter((cn) => cn == no).length;
		if (found == 0) {
			return -1;
		} else {
			return 0;
		}
	}

	export let btnCheckedClick = () => {};

	function inputKeyUp(e: any) {
		if(e.keyCode == 8){
			return;
		}
		
		let currentComponentId: number = +e.target.id;
		try {
			document.getElementById(rowNumber + "" + ((currentComponentId % 10) + 1)).focus();
		} catch (e) {
			document.getElementById(rowNumber+"").focus();
		}
	}

	function frmGuessSubmit() {
		checked = true;
		let countGreen: number = 0;
		for (let i: number = 0; i < count; i++) {
			let ch: number = checkGuess(+inputNumbers[i].value, i);
			if (ch == 1) {
				countGreen++;
				inputNumbers[i].style.backgroundColor = "green";
				inputNumbers[i].style.color = "khaki";
			} else if (ch == 0) {
				inputNumbers[i].style.backgroundColor = "yellow";
			} else {
				inputNumbers[i].style.backgroundColor = "red";
			}
			inputNumbers[i].disabled = true;
		}
		winner = countGreen == count;
		btnCheckedClick();
	}

	onMount(() => {
		count = inputNumbers.length;
	});
</script>

{#if display == true}
	<div class="card m-auto my-3 shadow px-5 rounded-pill">
		<card-body>
			<form on:submit|preventDefault={frmGuessSubmit}>
				<div class="row">
					<div class="col d-flex align-items-center">
						<input class="form-control  form-control-lg rounded-circle" style="width: 50px;text-align: center;" type="text" bind:value={rowNumber} disabled />
						<div class="vr mx-3" />
					</div>

					{#each inputNumbers as inputNumber, i}
						<div class="col d-flex justify-content-center">
							<input
							tabindex={i}
								id={rowNumber + "" + i}
								bind:this={inputNumber}
								on:keyup={inputKeyUp}
								class="form-control form-control-lg  my-3"
								style="width: 50px;text-align: center;"
								type="number"
								max="9"
								min="0"
								aria-label=".form-control-lg example"
								required
							/>
						</div>
					{/each}
					{#if !checked}
						<div class="col d-flex align-items-center">
							<button id={rowNumber+""} type="submit" class="btn btn-lg btn-outline-info"><i class="bi bi-check2" /></button>
						</div>
					{/if}
				</div>
			</form>
		</card-body>
	</div>
{/if}

<style>
	/* Chrome, Safari, Edge, Opera */
	input::-webkit-outer-spin-button,
	input::-webkit-inner-spin-button {
		-webkit-appearance: none;
		margin: 0;
	}

	/* Firefox */
	input[type="number"] {
		-moz-appearance: textfield;
	}
</style>
