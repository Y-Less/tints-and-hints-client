<script lang="ts">
	let { name } = $props();
	
	//let yourTurn = true;
	
	// This is a "rune", a part of the svelte language that tells it that this gets updated.
	// OK, clearly the documentation is out of date, because that says to use `$state(0)`.
	let count = $state(1);
	
	function increment()
	{
		count = count + 1;
	}

	function isYourTurn()
	{
		return count > 5;
	}
</script>

<main>
	{#if isYourTurn()}
		<h1>Try to give a hint.</h1>
		<div class="row">
			<div class="colour choice">Your tint</div><div class="colour hint" style="background-color: #00C000">B1</div>
		</div>
	{:else}
		<h1>Try to guess the tint.</h1>
		<div class="row">
			<div class="colour choice">1st choice</div><div class="colour hint" style="background-color: #00C000">B2</div>
			<div class="colour choice">2nd choice</div><div class="colour hint" style="background-color: #00C000">B2</div>
		</div>
	{/if}

	{count}
	<button onclick={increment}>hi</button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

	html
	{
		font-family: arial;
	}
	
	body
	{
		padding: 0;
		margin: 0;
	}
	
	#colour_grid
	{
		display: flex;
		height: 100%;
		width: 100%;
		flex-direction: column;
	}

	.row
	{
		display: flex;
		width: 100%;
	}

	.colour
	{
		height: 100px;
		width: 100px;
	}

	.top, .left, .hint, .choice
	{
		height: 100px;
		width: 100px;
		vertical-align: middle;
		text-align: center;
		line-height: 100px;
		font-size: 4em;
	}
	
	.choice
	{
		font-size: 2em;
		line-height: 1.5em;
		padding: 0 10px 0 10px;
	}

	body
	{
		display: grid;
		height: 100%;
		width: 100%;
	}
	
	.selected
	{
		width: 100%;
		height: 100%;
		border: 5px solid white;
		box-sizing: border-box;
	}
</style>

