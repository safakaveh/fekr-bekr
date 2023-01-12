<script lang="ts">
	import GameConfiguration from "$components/GameConfiguration.svelte";
	import GamePanel from "$components/GamePanel.svelte";

	let gameConfig: { randomCount?: number; guessCount?: number } = {};
	let gameStatus: { start?: boolean; end?: boolean; winner?: boolean; losser?: boolean } = { start: false, end: false };
	function frmStartGameSubmit() {
		gameStatus.start = true;
	}
</script>

{#if gameStatus.start == false}
	<GameConfiguration bind:gameConfig {frmStartGameSubmit} />
{:else}
	<GamePanel bind:gameConfig bind:gameStatus />
{/if}
