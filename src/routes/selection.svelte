<script lang="ts">
	import Selection from './selection.svelte';

	let { guesses, index } = $props();
</script>

{#if index < guesses.length}
<div class={guesses[index].first ? 'first-selection' : 'second-selection'} style="border-color: {guesses[index].colour}">
	<Selection guesses={guesses} index={index + 1} />
</div>
{/if}

<style>
	.first-selection, .second-selection
	{
		width: 100%;
		height: 100%;
		border: 5px dotted white;
		box-sizing: border-box;
	}

	.second-selection
	{
		border: 5px solid white;
	}
</style>

