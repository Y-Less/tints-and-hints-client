<script lang="ts">
	import Selection from './selection.svelte';

	const HOST = 'http://localhost';

	let gName = $state(''); // @hmr:keep
	let gColour = $state(''); // @hmr:keep
	let gTint = $state('??'); // @hmr:keep

	let gFirstGuesses = $state([]); // @hmr:keep
	let gSecondGuesses = $state(null); // @hmr:keep

	let gCurrentPlayer = $state(null); // @hmr:keep

	let gFirstGuess = $state('??'); // @hmr:keep
	let gSecondGuess = $state('??'); // @hmr:keep

	let gID = $state(-1); // @hmr:keep

	let gTimer = $state(null); // @hmr:keep

	let gPlayers = $state([]); // Don't keep.

	const
		GRID = {
			A: [ 'hsl(  0deg   25%   30%)', 'hsl(  0deg   50%   30%)', 'hsl(  0deg   75%   30%)', 'hsl(  0deg  100%   30%)', 'hsl(  0deg  100%   40%)', 'hsl(  0deg  100%   50%)', 'hsl(  0deg  100%   60%)', 'hsl(  0deg  100%   70%)', 'hsl(  0deg  100%   80%)', 'hsl(  0deg  100%   90%)' ],
			B: [ 'hsl( 15deg   25%   30%)', 'hsl( 15deg   50%   30%)', 'hsl( 15deg   75%   30%)', 'hsl( 15deg  100%   30%)', 'hsl( 15deg  100%   40%)', 'hsl( 15deg  100%   50%)', 'hsl( 15deg  100%   60%)', 'hsl( 15deg  100%   70%)', 'hsl( 15deg  100%   80%)', 'hsl( 15deg  100%   90%)' ],
			C: [ 'hsl( 30deg   25%   30%)', 'hsl( 30deg   50%   30%)', 'hsl( 30deg   75%   30%)', 'hsl( 30deg  100%   30%)', 'hsl( 30deg  100%   40%)', 'hsl( 30deg  100%   50%)', 'hsl( 30deg  100%   60%)', 'hsl( 30deg  100%   70%)', 'hsl( 30deg  100%   80%)', 'hsl( 30deg  100%   90%)' ],
			D: [ 'hsl( 45deg   25%   30%)', 'hsl( 45deg   50%   30%)', 'hsl( 45deg   75%   30%)', 'hsl( 45deg  100%   30%)', 'hsl( 45deg  100%   40%)', 'hsl( 45deg  100%   50%)', 'hsl( 45deg  100%   60%)', 'hsl( 45deg  100%   70%)', 'hsl( 45deg  100%   80%)', 'hsl( 45deg  100%   90%)' ],
			E: [ 'hsl( 60deg   25%   30%)', 'hsl( 60deg   50%   30%)', 'hsl( 60deg   75%   30%)', 'hsl( 60deg  100%   30%)', 'hsl( 60deg  100%   40%)', 'hsl( 60deg  100%   50%)', 'hsl( 60deg  100%   60%)', 'hsl( 60deg  100%   70%)', 'hsl( 60deg  100%   80%)', 'hsl( 60deg  100%   90%)' ],
			F: [ 'hsl( 75deg   25%   30%)', 'hsl( 75deg   50%   30%)', 'hsl( 75deg   75%   30%)', 'hsl( 75deg  100%   30%)', 'hsl( 75deg  100%   40%)', 'hsl( 75deg  100%   50%)', 'hsl( 75deg  100%   60%)', 'hsl( 75deg  100%   70%)', 'hsl( 75deg  100%   80%)', 'hsl( 75deg  100%   90%)' ],
			G: [ 'hsl( 90deg   25%   30%)', 'hsl( 90deg   50%   30%)', 'hsl( 90deg   75%   30%)', 'hsl( 90deg  100%   30%)', 'hsl( 90deg  100%   40%)', 'hsl( 90deg  100%   50%)', 'hsl( 90deg  100%   60%)', 'hsl( 90deg  100%   70%)', 'hsl( 90deg  100%   80%)', 'hsl( 90deg  100%   90%)' ],
			H: [ 'hsl(105deg   25%   30%)', 'hsl(105deg   50%   30%)', 'hsl(105deg   75%   30%)', 'hsl(105deg  100%   30%)', 'hsl(105deg  100%   40%)', 'hsl(105deg  100%   50%)', 'hsl(105deg  100%   60%)', 'hsl(105deg  100%   70%)', 'hsl(105deg  100%   80%)', 'hsl(105deg  100%   90%)' ],
			I: [ 'hsl(120deg   25%   30%)', 'hsl(120deg   50%   30%)', 'hsl(120deg   75%   30%)', 'hsl(120deg  100%   30%)', 'hsl(120deg  100%   40%)', 'hsl(120deg  100%   50%)', 'hsl(120deg  100%   60%)', 'hsl(120deg  100%   70%)', 'hsl(120deg  100%   80%)', 'hsl(120deg  100%   90%)' ],
			J: [ 'hsl(135deg   25%   30%)', 'hsl(135deg   50%   30%)', 'hsl(135deg   75%   30%)', 'hsl(135deg  100%   30%)', 'hsl(135deg  100%   40%)', 'hsl(135deg  100%   50%)', 'hsl(135deg  100%   60%)', 'hsl(135deg  100%   70%)', 'hsl(135deg  100%   80%)', 'hsl(135deg  100%   90%)' ],
			K: [ 'hsl(150deg   25%   30%)', 'hsl(150deg   50%   30%)', 'hsl(150deg   75%   30%)', 'hsl(150deg  100%   30%)', 'hsl(150deg  100%   40%)', 'hsl(150deg  100%   50%)', 'hsl(150deg  100%   60%)', 'hsl(150deg  100%   70%)', 'hsl(150deg  100%   80%)', 'hsl(150deg  100%   90%)' ],
			L: [ 'hsl(165deg   25%   30%)', 'hsl(165deg   50%   30%)', 'hsl(165deg   75%   30%)', 'hsl(165deg  100%   30%)', 'hsl(165deg  100%   40%)', 'hsl(165deg  100%   50%)', 'hsl(165deg  100%   60%)', 'hsl(165deg  100%   70%)', 'hsl(165deg  100%   80%)', 'hsl(165deg  100%   90%)' ],
			M: [ 'hsl(180deg   25%   30%)', 'hsl(180deg   50%   30%)', 'hsl(180deg   75%   30%)', 'hsl(180deg  100%   30%)', 'hsl(180deg  100%   40%)', 'hsl(180deg  100%   50%)', 'hsl(180deg  100%   60%)', 'hsl(180deg  100%   70%)', 'hsl(180deg  100%   80%)', 'hsl(180deg  100%   90%)' ],
			N: [ 'hsl(195deg   25%   30%)', 'hsl(195deg   50%   30%)', 'hsl(195deg   75%   30%)', 'hsl(195deg  100%   30%)', 'hsl(195deg  100%   40%)', 'hsl(195deg  100%   50%)', 'hsl(195deg  100%   60%)', 'hsl(195deg  100%   70%)', 'hsl(195deg  100%   80%)', 'hsl(195deg  100%   90%)' ],
			O: [ 'hsl(210deg   25%   30%)', 'hsl(210deg   50%   30%)', 'hsl(210deg   75%   30%)', 'hsl(210deg  100%   30%)', 'hsl(210deg  100%   40%)', 'hsl(210deg  100%   50%)', 'hsl(210deg  100%   60%)', 'hsl(210deg  100%   70%)', 'hsl(210deg  100%   80%)', 'hsl(210deg  100%   90%)' ],
			P: [ 'hsl(225deg   25%   30%)', 'hsl(225deg   50%   30%)', 'hsl(225deg   75%   30%)', 'hsl(225deg  100%   30%)', 'hsl(225deg  100%   40%)', 'hsl(225deg  100%   50%)', 'hsl(225deg  100%   60%)', 'hsl(225deg  100%   70%)', 'hsl(225deg  100%   80%)', 'hsl(225deg  100%   90%)' ],
			Q: [ 'hsl(240deg   25%   30%)', 'hsl(240deg   50%   30%)', 'hsl(240deg   75%   30%)', 'hsl(240deg  100%   30%)', 'hsl(240deg  100%   40%)', 'hsl(240deg  100%   50%)', 'hsl(240deg  100%   60%)', 'hsl(240deg  100%   70%)', 'hsl(240deg  100%   80%)', 'hsl(240deg  100%   90%)' ],
			R: [ 'hsl(255deg   25%   30%)', 'hsl(255deg   50%   30%)', 'hsl(255deg   75%   30%)', 'hsl(255deg  100%   30%)', 'hsl(255deg  100%   40%)', 'hsl(255deg  100%   50%)', 'hsl(255deg  100%   60%)', 'hsl(255deg  100%   70%)', 'hsl(255deg  100%   80%)', 'hsl(255deg  100%   90%)' ],
			S: [ 'hsl(270deg   25%   30%)', 'hsl(270deg   50%   30%)', 'hsl(270deg   75%   30%)', 'hsl(270deg  100%   30%)', 'hsl(270deg  100%   40%)', 'hsl(270deg  100%   50%)', 'hsl(270deg  100%   60%)', 'hsl(270deg  100%   70%)', 'hsl(270deg  100%   80%)', 'hsl(270deg  100%   90%)' ],
			T: [ 'hsl(285deg   25%   30%)', 'hsl(285deg   50%   30%)', 'hsl(285deg   75%   30%)', 'hsl(285deg  100%   30%)', 'hsl(285deg  100%   40%)', 'hsl(285deg  100%   50%)', 'hsl(285deg  100%   60%)', 'hsl(285deg  100%   70%)', 'hsl(285deg  100%   80%)', 'hsl(285deg  100%   90%)' ],
			U: [ 'hsl(300deg   25%   30%)', 'hsl(300deg   50%   30%)', 'hsl(300deg   75%   30%)', 'hsl(300deg  100%   30%)', 'hsl(300deg  100%   40%)', 'hsl(300deg  100%   50%)', 'hsl(300deg  100%   60%)', 'hsl(300deg  100%   70%)', 'hsl(300deg  100%   80%)', 'hsl(300deg  100%   90%)' ],
			V: [ 'hsl(315deg   25%   30%)', 'hsl(315deg   50%   30%)', 'hsl(315deg   75%   30%)', 'hsl(315deg  100%   30%)', 'hsl(315deg  100%   40%)', 'hsl(315deg  100%   50%)', 'hsl(315deg  100%   60%)', 'hsl(315deg  100%   70%)', 'hsl(315deg  100%   80%)', 'hsl(315deg  100%   90%)' ],
			W: [ 'hsl(330deg   25%   30%)', 'hsl(330deg   50%   30%)', 'hsl(330deg   75%   30%)', 'hsl(330deg  100%   30%)', 'hsl(330deg  100%   40%)', 'hsl(330deg  100%   50%)', 'hsl(330deg  100%   60%)', 'hsl(330deg  100%   70%)', 'hsl(330deg  100%   80%)', 'hsl(330deg  100%   90%)' ],
			X: [ 'hsl(345deg   25%   30%)', 'hsl(345deg   50%   30%)', 'hsl(345deg   75%   30%)', 'hsl(345deg  100%   30%)', 'hsl(345deg  100%   40%)', 'hsl(345deg  100%   50%)', 'hsl(345deg  100%   60%)', 'hsl(345deg  100%   70%)', 'hsl(345deg  100%   80%)', 'hsl(345deg  100%   90%)' ],
		},
		COLS = Object.keys(GRID),
		ROWS = Object.keys(GRID.A);

	function stringToColour(str)
	{
		if (str === '??' || str === '')
		{
			return '#FFFFFF';
		}
		return GRID[str[0]][str[1]];
	}

	function get(url)
	{
		return new Promise(function (resolve, reject)
		{
			fetch(HOST + ':3000' + url, { method: 'GET' }).then(function (response)
			{
				if (!response.ok) reject(`Response status: ${response.status}`);
				response.json().then(function (json) { resolve(json); });
			});
		});
	}

	function post(url, body)
	{
		return new Promise(function (resolve, reject)
		{
			fetch(HOST + ':3000' + url, {
				method: 'POST',
				body: JSON.stringify(body),
				headers: { 'Content-Type': 'application/json' },
			}).then(function (response)
			{
				if (!response.ok) reject(`Response status: ${response.status}`);
				response.json().then(function (json) { resolve(json); });
			});
		});
	}

	function updateState(state)
	{
		const { failed } = state;
		if (failed != null)
		{
			console.error(failed);
			return;
		}
		const {
			first,
			second,
			current,
			id,
			players,
			tint,
		} = state;
		gFirstGuesses = first;
		gSecondGuesses = second;
		gPlayers = players;
		gID = id;
		gTint = tint;
		gCurrentPlayer = (current === -1) ? null : gPlayers[current];
		gFirstGuess = '??';
		for (const i of gFirstGuesses)
		{
			if (i.name === gName)
			{
				gFirstGuess = i.guess;
				break;
			}
		}
		gSecondGuess = '??';
		if (gSecondGuesses != null)
		{
			for (const i of gSecondGuesses)
			{
				if (i.name === gName)
				{
					gSecondGuess = i.guess;
					break;
				}
			}
		}
	}

	function allDone()
	{
		post('/api/next', { name: gName });
	}

	function guess(row, col)
	{
		const guess = `${col}${row}`;
		return function()
		{
			if (gColour === '')
			{
				// First selection, to choose a player colour.
				gColour = GRID[col][row];
				post('/api/add-player', { name: gName, colour: gColour }).then(updateState);

				if (gTimer != null)
				{
					clearInterval(gTimer);
				}
				gTimer = setInterval(function()
				{
					get('/api/poll?name=' + gName).then(updateState);
				}, 500);
			}
			else
			{
				post('/api/guess', { guess, name: gName }).then(updateState);
			}
		}
	}

	function getGuesses(row, col)
	{
		const addr = `${col}${row}`;
		const ret = [];
		for (const g of gFirstGuesses)
		{
			if (g.guess === addr)
			{
				ret.push(g);
			}
		}
		if (gSecondGuesses != null)
		{
			for (const g of gSecondGuesses)
			{
				if (g.guess === addr)
				{
					ret.push(g);
				}
			}
		}
		return ret;
	}

	function setName(event)
	{
		if (event.keyCode === 13 || event.type === 'click')
		{
			// Pressed enter in the text box.
			gName = document.getElementById('enter-input').value?.trim() ?? '';
			get('/api/poll').then(updateState);
		}
	}

</script>

<main>
{#if gName === ''}
	<h1 id="title">Enter your name.</h1>
	<div id="enter-name">
		<input id="enter-input" type="text" onkeypress={setName} />
		<button onclick={setName}>OK</button>
	</div>
{:else}
	{#if gColour === ''}
	<h1 id="title">Pick your player colour.</h1>
	<div id="status" class="row">
		<div class="colour hint" style="background-color: #FFFFFF"></div>
	</div>
	{:else if gCurrentPlayer == null}
	<h1 id="title">Wait for other players.</h1>
	<div id="status" class="row">
		<div class="colour hint" style="background-color: #FFFFFF"></div>
	</div>
	{:else if gTint === '??'}
	<h1 id="title">Try to guess the tint.</h1>
	<div id="status" class="row">
		<div class="colour">1st</div><div class="colour hint" style="background-color: {stringToColour(gFirstGuess)}">{gFirstGuess}</div>
		{#if gSecondGuesses != null}
		<div class="colour">2nd</div><div class="colour hint" style="background-color: {stringToColour(gSecondGuess)}">{gSecondGuess}</div>
		{/if}
	</div>
	{:else}
	<h1 id="title">Try to give a hint.</h1>
	<div id="status" class="row">
		<div class="colour">Your tint</div><div class="colour hint" style="background-color: {stringToColour(gTint)}">{gTint}</div>
		<button onclick={allDone}>OK</button>
	</div>
	{/if}

	<table id="colour-grid">
		<tbody>
			<tr style="height: {100 / (ROWS.length + 1)}%">
				<th style="width: {100 / (COLS.length + 1)}%"></th>
{#each COLS as j}
				<th style="width: {100 / (COLS.length + 1)}%">{j}</th>
{/each}
			</tr>
{#each ROWS as i}
			<tr style="height: {100 / (ROWS.length + 1)}%">
				<th>{i}</th>
	{#each COLS as j}
				<td style="background-color: {GRID[j][i]}" onclick={guess(i, j)}>
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
{#each gPlayers as i}
				<tr>
					<th style="background-color: {i.colour}"></th>
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
		height: 100%;
		text-align: center;
		display: grid;
		grid-template-columns: 1fr 300px 300px;
		grid-template-rows: 120px 1fr 10px;
		
		grid-template-areas:
			"title status status"
			"main main sidebar"
			"footer footer footer";
	}

	#title
	{
		grid-area: title;
	}

	#status, #enter-name
	{
		grid-area: status;
		margin: 20px 20px 0 0;
	}

	h1
	{
		color: #FF3E00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		margin-top: 35px;
	}

	#colour-grid
	{
		height: 100%;
		width: 100%;
		table-layout: fixed;
		grid-area: main;
		border-spacing: 0;
	}

	#colour-grid td, #colour-grid th
	{
		border: 0;
	}

	#colour-grid tbody :first-child :not(:first-child)
	{
		border-bottom: 10px solid black;
	}

	#colour-grid tbody :not(:first-child) :first-child
	{
		border-right: 10px solid black;
	}

	#colour-grid tbody :last-child :not(:first-child)
	{
		border-bottom: 10px solid black;
	}

	#colour-grid tbody :not(:first-child) :last-child
	{
		border-right: 10px solid black;
	}

	#player-grid
	{
		grid-area: sidebar;
		align-self: start;
	}

	#player-grid table
	{
		table-layout: fixed;
		width: 280px;
		margin-left: 20px;
	}

	#player-grid th
	{
		width: 30px;
		height: 30px;
	}

	#enter-name
	{
		text-align: center;
	}
	
	button
	{
		max-height: 100px;
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

