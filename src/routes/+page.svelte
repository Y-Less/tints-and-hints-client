<script lang="ts">
	import Selection from './selection.svelte';

	let { name } = $props();
	
	//let yourTurn = true;
	
	// This is a "rune", a part of the svelte language that tells it that this gets updated.
	// OK, clearly the documentation is out of date, because that says to use `$state(0)`.
	let count = $state(1);
	
	let guesses = $state([]);
	
	function increment()
	{
		count = count + 1;
	}

	function isYourTurn()
	{
		return count > 5;
	}
	
	const
		GRID = {
			A: ['D0EDD4', '14FAEB', '886C92', 'E72866', '002020', '71AFD7', '7476F0'],
			B: ['CAEBCF', '1EF8E1', '81728C', 'E8266E', '002626', '77B2D9', '6D7CEF'],
			C: ['C5E9CA', '28F6D7', '7B7886', 'E92377', '002B2B', '7CB5DA', '6782EF'],
			D: ['C0E7C5', '33F3CD', '747E80', 'EB217F', '003030', '82B8DC', '6087EE'],
			E: ['BBE5C1', '3DF1C3', '6D847A', 'EC1F88', '003535', '87BBDD', '5A8DED'],
			F: ['B6E3BC', '47EFB9', '678A74', 'ED1D90', '003B3B', '8DBEDF', '5393ED'],
			G: ['B0E1B7', '51EDAF', '60906E', 'EF1A99', '004040', '92C1E0', '4C98EC'],
			H: ['ABDFB2', '5BEAA5', '5A9668', 'F018A1', '004545', '97C4E2', '469EEB'],
			I: ['A6DDAE', '66E89B', '539C62', 'F116AA', '004B4B', '9DC7E3', '3FA4EB'],
			J: ['A1DBA9', '70E691', '4CA25C', 'F314B2', '005050', 'A2CAE5', '39A9EA'],
			K: ['9CDAA4', '7AE487', '46A856', 'F411BB', '005555', 'A8CDE6', '32AFE9'],
			L: ['96D89F', '84E17D', '3FAE50', 'F50FC3', '005A5A', 'ADD1E8', '2CB5E9'],
			M: ['91D69A', '8EDF73', '39B54A', 'F70DCC', '006060', 'B3D4E9', '2CB5E1'],
			N: ['8CD496', '99DD69', '42B846', 'F80BD4', '006565', 'B8D7EB', '2DB5D9'],
			O: ['87D291', 'A3DB5F', '4BBB43', 'F908DD', '006A6A', 'BDDAEC', '2DB5D2'],
			P: ['81D08C', 'ADD855', '54BF3F', 'FB06E5', '007070', 'C3DDEE', '2EB5CA'],
			Q: ['7CCE87', 'B7D64B', '5DC23C', 'FC04EE', '007575', 'C8E0EF', '2FB5C3'],
			R: ['77CC83', 'C1D441', '66C539', 'FD02F6', '007A7A', 'CEE3F1', '2FB5BB'],
			S: ['72CA7E', 'CCD237', '6FC935', 'FF00FF', '008080', 'D3E6F2', '30B5B4'],
			T: ['6DC879', 'D6CF2D', '78CC32', 'F300FF', '008585', 'D9E9F4', '30B5AC'],
			U: ['67C674', 'E0CD23', '81CF2F', 'E700FF', '008A8A', 'DEECF5', '31B5A4'],
			V: ['62C470', 'EACB19', '8AD32B', 'DC00FF', '008F8F', 'E3EFF7', '32B59D'],
			W: ['5DC26B', 'F4C90F', '93D628', 'D000FF', '009595', 'E9F2F8', '32B595'],
			X: ['58C066', 'FFC706', '9CDA25', 'C500FF', '009A9A', 'EEF5FA', '33B58E'],
			Y: ['53BE61', 'FEC709', 'A5DD21', 'B900FF', '009F9F', 'F4F8FB', '34B586'],
			Z: ['4DBC5D', 'FDC70D', 'AEE01E', 'AD00FF', '00A5A5', 'F9FBFD', '34B57F'],
		},
		COLS = Object.keys(GRID),
		ROWS = Object.keys(GRID.A);

	function guess(row, col)
	{
		const addr = `${col}${row}`;
		return function()
		{
			// Clicked.
			console.log(`You clicked on ${addr}`);
		}
	}
	
	function getGuesses(row, col)
	{
		const addr = `${col}${row}`;
		const ret = [];
		for (const g of guesses)
		{
			if (g.guess === addr)
			{
				ret.push(g);
			}
		}
		return ret;
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
	
	<div id="colour_grid">
		<div class="row">
			<div class="left"></div>
{#each COLS as j}
			<div class="top">{j}</div>
{/each}
		</div>
{#each ROWS as i}
		<div class="row">
			<div class="left">{i}</div>
	{#each COLS as j}
			<div class="colour" style="background-color: #{GRID[j][i]}" onclick={guess(i, j)}>
				<Selection guesses={getGuesses(i, j)} index={0} />
			</div>
	{/each}
		</div>
{/each}
	</div>
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

	main
	{
		display: grid;
		height: 100%;
		width: 100%;
		font-family: arial;
		padding: 0;
		margin: 0;
	}
</style>

