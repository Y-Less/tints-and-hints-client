<script lang="ts">
	import Selection from './selection.svelte';
	
	//let yourTurn = true;
	
	// This is a "rune", a part of the svelte language that tells it that this gets updated.
	// OK, clearly the documentation is out of date, because that says to use `$state(0)`.
	let count = $state(1);
	
	let name = $state('');
	let colour = $state('');
	
	let firstGuesses = $state([
		{ name: 'Alex', colour: '000000', first: true, guess: 'B5' },
		{ name: 'Alex', colour: '000000', first: true, guess: 'B7' },
		{ name: 'Alex', colour: '000000', first: true, guess: 'C3' },
		{ name: 'Alex', colour: '000000', first: true, guess: 'E5' },
		{ name: 'Alex', colour: '00FF00', first: true, guess: 'E5' },
	]);
	
	let secondGuesses = $state([
		{ name: 'Alex', colour: '0000FF', first: false, guess: 'E5' },
		{ name: 'Alex', colour: 'FF0000', first: false, guess: 'E5' },
		{ name: 'Alex', colour: '000000', first: false, guess: 'G5' },
	]);
	
	function increment()
	{
		count = count + 1;
	}

	function isYourTurn()
	{
		return count > 5;
	}
	
	let players = $state([
		{
			name: 'Alex',
			score: 1000,
			colour: 'FF0000',
		},
		{
			name: 'Tom',
			score: 1000,
			colour: '00FF00',
		},
	]);
	
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
			if (colour === '')
			{
				// First selection, to choose a player colour.
				colour = GRID[col][row];
			}
			else
			{
				// Clicked.
				console.log(`You clicked on ${addr}`);
			}
		}
	}
	
	function getGuesses(row, col)
	{
		const addr = `${col}${row}`;
		const ret = [];
		for (const g of firstGuesses)
		{
			if (g.guess === addr)
			{
				ret.push(g);
			}
		}
		for (const g of secondGuesses)
		{
			if (g.guess === addr)
			{
				ret.push(g);
			}
		}
		return ret;
	}
	
	function setName(event)
	{
		if (event.keyCode === 13 || event.type === 'click')
		{
			// Pressed enter in the text box.
			name = document.getElementById('enter-input').value?.trim() ?? '';
		}
	}
	
</script>

<main>
{#if name === ''}
	<div id="header">
		<h1>Enter your name.</h1>
		<div id="enter-name">
			<input id="enter-input" type="text" onkeypress={setName} />
			<button onclick={setName}>OK</button>
		</div>
	</div>
{:else}
	<div id="header">
	{#if colour === ''}
		<h1>Pick your player colour.</h1>
	{:else}
		{#if isYourTurn()}
		<h1>Try to give a hint.</h1>
		<div class="row">
			<div class="colour">Your tint</div><div class="colour hint" style="background-color: #00C000">B1</div>
		</div>
		{:else}
		<h1>Try to guess the tint.</h1>
		{/if}
	{/if}
	</div>
	
	<table id="colour-grid">
		<tbody>
			<tr>
				<th style="width: {100 / (COLS.length + 1)}%"></th>
{#each COLS as j}
				<th style="width: {100 / (COLS.length + 1)}%">{j}</th>
{/each}
			</tr>
{#each ROWS as i}
			<tr>
				<th>{i}</th>
	{#each COLS as j}
				<td style="background-color: #{GRID[j][i]}" onclick={guess(i, j)}>
					<Selection guesses={getGuesses(i, j)} index={0} />
				</td>
	{/each}
			</tr>
{/each}
		</tbody>
	</table>

	<div id="player-grid">
		<h2>Scores</h2>
		<table>
			<tbody>
{#each players as i}
				<tr>
					<th style="background-color: #{i.colour}"></th>
					<td>{i.name}</td>
					<td>{i.score}</td>
				</tr>
{/each}
			</tbody>
		</table>
	</div>
{/if}
</main>

<style>
	main
	{
		text-align: center;
		display: grid;
		grid-template-columns: 1fr 400px;
		
		grid-template-areas: 
			"header header"
			"main sidebar"
			"footer footer";
	}
	
	#header
	{	
		grid-area: header;
	}

	h1
	{
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	#colour-grid
	{
		height: 100%;
		width: 100%;
		table-layout: fixed;
		border-collapse: collapse;
		grid-area: main;
	}

	#player-grid
	{
		grid-area: sidebar;
		align-self: start;
	}
	
	#player-grid table
	{
		table-layout: fixed;
		width: 320px;
		margin-left: 40px;
	}
	
	#player-grid th
	{
		width: 30px;
		height: 30px;
	}
	
	#colour-grid tr
	{
		height: 50px;
	}
	
	#enter-name
	{
		text-align: center;
	}
	
	input, button
	{
		max-width: 500px;
		font-size: 3em;
		text-align: center;
	}

	#colour-grid th
	{
		vertical-align: middle;
		text-align: center;
		line-height: 100%;
		font-size: 2em;
	}
	
	.colour
	{
		font-size: 2em;
		line-height: 1.5em;
		padding: 0 10px 0 10px;
		width: 100px;
		height: 100px;
	}
	
	.row
	{
		display: flex;
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

