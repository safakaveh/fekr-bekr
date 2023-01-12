<script lang="ts">
	export let gameConfig: { randomCount?: number; guessCount?: number } = {};
	export let frmStartGameSubmit = () => {};
</script>

<form on:submit|preventDefault={frmStartGameSubmit} class="was-validated">
	<div class="card w-50 m-auto mt-5 rounded shadow">
		<div class="card-body d-flex justify-content-center">
			<div class="row ">
				<div class="col">
					<input
						bind:value={gameConfig.randomCount}
						type="number"
						max="10"
						min="3"
						class="form-control"
						placeholder="Number of random numbers"
						aria-label="First name"
						required
					/>
				</div>
				<div class="col">
					<input
						bind:value={gameConfig.guessCount}
						type="number"
						max="20"
						min="2"
						class="form-control"
						placeholder="Number of guess lines"
						aria-label="Last name"
						required
					/>
				</div>
				<div class="col-2">
					<button type="submit" class="btn btn-primary">Start</button>
				</div>
			</div>
		</div>
	</div>
</form>
