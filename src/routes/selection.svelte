<script lang="ts">
	import Selection from './selection.svelte';

	let { guesses, index } = $props();
</script>

{#if index < guesses.length}
<div class="selected">
	<Selection guesses={guesses} index={index + 1} />
</div>
{/if}

<style>
	.selected
	{
		width: 100%;
		height: 100%;
		border: 5px solid white;
		box-sizing: border-box;
	}
</style>

