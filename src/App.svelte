<script>
	import SwipeListener from 'swipe-listener';
	import Cell from './Cell.svelte';

	const gridSize = 20

	let score = 0

	let snake = [[10, 10], [10, 11], [10, 12], [10, 13]]
	let direction = 'LEFT'
	let foodSpot = randomFoodSpot(gridSize, snake)

	let interval = 500;
	let ticker = setInterval(tick, interval)

	function reset() {
		score = 0
		snake = [[10, 10], [10, 11], [10, 12], [10, 13]]
		direction = 'LEFT'
		foodSpot = randomFoodSpot(gridSize, snake)
		clearInterval(ticker)
		interval = 500;
		ticker = setInterval(tick, interval)
	}

	function tick() {
		let hasEaten = false
		let [headRow, headCol] = snake[0]
		let newHead;
		switch (direction) {
			case 'RIGHT':
				newHead = [headRow, headCol + 1]
				break;
			case 'DOWN':
				newHead = [headRow + 1, headCol]
				break;
			case 'LEFT':
				newHead = [headRow, headCol - 1]
				break;
			case 'UP':
				newHead = [headRow - 1, headCol]
				break;
		}

		if (newHead[0] === foodSpot[0] && newHead[1] === foodSpot[1]) {
			hasEaten = true
		} else if (
			newHead[0] < 0 ||
			newHead[0] === rows.length ||
			newHead[1] < 0 ||
			newHead[1] === cols.length ||
			snake.some(piece => piece[0] === newHead[0] && piece[1] === newHead[1])
		) {
			reset();
			return
		}
		snake = [newHead, ...snake.slice(0, hasEaten ? snake.length : snake.length - 1)]
		if (hasEaten) {
			score++
			foodSpot = randomFoodSpot(gridSize, snake)
			clearInterval(ticker)
			interval /= 1.1
			ticker = setInterval(tick, interval)
		}
	}

	let rows = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19];
	let cols = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19];

	function handleKeydown(event) {
		const keyCode = event.keyCode
		switch (keyCode) {
			case 37:
				direction = 'LEFT'
				break;
			case 38:
				direction = 'UP'
				break;
			case 39:
				direction = 'RIGHT'
				break;
			case 40:
				direction = 'DOWN'
				break;
		}
	}

	function handleSwipe(event) {
		const directions = event.detail.directions
		if (directions.top) {
			direction = 'UP'
		} else if (directions.bottom) {
			direction = 'DOWN'
		} else if (directions.left) {
			direction = 'LEFT'
		} else if (directions.right) {
			direction = 'RIGHT'
		}
	}

	SwipeListener(window)

	function randomFoodSpot(gridSize, snake) {
		let randomRow = Math.floor(Math.random() * gridSize)
		let randomCol = Math.floor(Math.random() * gridSize)
		if (snake.some(([row, col]) => row === randomRow && col === randomCol)) {
			return randomFoodSpot(gridSize, snake)
		} else {
			return [randomRow, randomCol]
		}
	}
</script>

<main>
	<div class="wrapper">
		<div class="scores">Score: {score}</div>
		<div class="grid">
			{#each rows as rowIndex}
				<div class="row">
				{#each cols as colIndex}
					<Cell {rowIndex} {colIndex} {snake} {foodSpot}/>
				{/each}
				</div>
			{/each}
		</div>
	</div>
</main>

<svelte:window on:keydown={handleKeydown} on:swipe={handleSwipe}/>

<style>
	main {
		background-color: #ddd;
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100%;
	}

	.wrapper {
		display: flex;
		flex-direction: column;
		justify-content: start;
	}

	.scores {
		font-weight: bold;
		padding: 10px 0;
	}

	.grid {
		background-color: #fff;
	}

	.row {
		display: flex;
	}
</style>
