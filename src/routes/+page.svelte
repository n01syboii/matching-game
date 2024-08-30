<script lang="ts">
	import { emoji } from './emojis'

	type State = 'start' | 'playing' | 'won' | 'lost' | 'paused'

	let state: State = 'start'
	let size = 20
	let grid = createGrid()
	let maxMatches = grid.length / 2
	let selected: number[] = []
	let matches: string[] = []
	let timerId: number | null = null
	let time = 60

	function startGameTimer() {
		function countdown() {
			state !== 'paused' && time--
		}
		timerId = setInterval(countdown, 1000)
	}

	function createGrid() {
		let cards = new Set<string>()
		let maxSize = size / 2

		while (cards.size < maxSize) {
			const randomIndex = Math.floor(Math.random() * emoji.length)
			cards.add(emoji[randomIndex])
		}

		return shuffle([...cards, ...cards])
	}

	function shuffle<Items>(array: Items[]) {
		return array.sort(() => Math.random() - 0.5)
	}

	function selectCard(cardIndex: number) {
		selected = selected.concat(cardIndex)
	}

	function matchCards() {
		const [first, second] = selected

		if (grid[first] === grid[second]) {
			matches = matches.concat(grid[first])
		}

		setTimeout(() => (selected = []), 300)
	}

	function resetGame() {
		timerId && clearInterval(timerId)
		grid = createGrid()
		maxMatches = grid.length / 2
		selected = []
		matches = []
		timerId = null
		time = 60
	}

	function gameWon() {
		state = 'won'
		resetGame()
	}
	function gameLost() {
		state = 'lost'
		resetGame()
	}

	function pauseGame(e: KeyboardEvent) {
		if (e.key === 'Escape')
			switch (state) {
				case 'playing':
					state = 'paused'
					break
				case 'paused':
					state = 'playing'
					break
			}
	}

	$: selected.length === 2 && matchCards()
	$: maxMatches === matches.length && gameWon()
	$: time === 0 && gameLost()
	$: if (state === 'playing') {
		!timerId && startGameTimer()
	}
	$: console.log({ state, selected, matches }, timerId)
</script>

<svelte:window on:keydown={pauseGame} />

{#if state === 'paused'}
	<h1>Game Paused</h1>
{/if}

{#if state === 'start'}
	<h1 class="start-heading">Matching Game</h1>
	<button class="play-button" on:click={() => (state = 'playing')}>Play</button>
{/if}

{#if state === 'playing'}
	<h1 class="timer" class:pulse={time <= 10}>
		{time}
	</h1>

	<div class="matches">
		{#each matches as cards}
			<div>{cards}</div>
		{/each}
	</div>

	<div class="cards">
		{#each grid as card, cardIndex}
			{@const isSelected = selected.includes(cardIndex)}
			{@const isSelectedOrMatched = selected.includes(cardIndex) || matches.includes(card)}
			{@const match = matches.includes(card)}

			<button
				class="card"
				class:selected={isSelected}
				class:flip={isSelectedOrMatched}
				disabled={isSelectedOrMatched}
				on:click={() => selectCard(cardIndex)}
			>
				<div class="back" class:match>{card}</div>
			</button>
		{/each}
	</div>
{/if}

{#if state === 'won'}
	<h1>You Win! 🥳</h1>

	<button class="play-button" on:click={() => (state = 'playing')}>Play again</button>
{/if}

{#if state === 'lost'}
	<h1>You Loose!! 💩</h1>

	<button class="play-button" on:click={() => (state = 'playing')}>Play again</button>
{/if}

<style>
	@keyframes slideInFromTop {
		0% {
			opacity: 0;
			transform: translateY(-150px);
		}
		100% {
			opacity: 1;
			transform: translateY(0);
		}
	}

	@keyframes slideInFromBelow {
		0% {
			opacity: 0;
			transform: translateY(150px);
		}
		100% {
			opacity: 1;
			transform: translateY(0);
		}
	}

	@keyframes fadeIn {
		0% {
			opacity: 0;
		}
		98%,
		100% {
			opacity: 1;
		}
	}

	@keyframes pulse {
		to {
			scale: 1.4;
		}
	}

	.start-heading {
		animation: slideInFromTop 1.5s ease-in-out;
	}

	.play-button {
		animation: slideInFromBelow 1.5s ease-in-out;
		padding: 1rem 3rem;
	}

	.cards {
		animation: fadeIn 1.5s ease-in;
		display: grid;
		grid-template-columns: repeat(5, 1fr);
		gap: 0.4rem;
	}

	.card {
		height: 140px;
		width: 140px;
		font-size: 4rem;
		background-color: var(--bg-2);
		transition: rotate 0.3s ease-out;
		transform-style: preserve-3d;

		&[disabled] {
			cursor: default;
		}

		&.flip {
			rotate: y 180deg;
			pointer-events: none;
		}

		& .back {
			position: absolute;
			inset: 0;
			display: grid;
			place-content: center;
			backface-visibility: hidden;
			rotate: y 180deg;
		}

		&.selected {
			border: 4px solid var(--border);
			transition: 0.1s ease-in;
			cursor: default;
		}

		& .match {
			transition: opacity 1s ease-out;
			opacity: 0.3;
			cursor: default;
		}
	}

	.matches {
		display: flex;
		gap: 1rem;
		margin-block: 2rem;
		font-size: 3rem;
	}

	.timer {
		animation: fadeIn 1.5s ease-in;
		transition: color 0.4s ease;
	}

	.pulse {
		color: var(--pulse);
		animation: pulse 1s infinite ease;
	}
</style>
