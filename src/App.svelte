<script>
	import Row from './Row.svelte';
	const gridSize = 20

	let snake = [[10, 10], [10, 11], [10, 12], [10, 13]]
	let direction = "NORTH"
	let foodSpot = randomFoodSpot(gridSize, snake)

	let interval = 500;
	let ticker = setInterval(tick, interval)

	function tick() {
		let hasEaten = false
		let [headRow, headCol] = snake[0]
		let newHead;
		switch (direction) {
			case 'EAST':
				newHead = [headRow, (headCol + 1 === gridSize) ? 0 : headCol + 1]
				break;
			case 'SOUTH':
				newHead = [(headRow + 1 === gridSize) ? 0 : headRow + 1, headCol]
				break;
			case 'WEST':
				newHead = [headRow, (headCol - 1 < 0) ? gridSize - 1 : headCol - 1]
				break;
			case 'NORTH':
				newHead = [(headRow - 1 < 0) ? gridSize - 1 : headRow - 1, headCol]
				break;
		}

		if (newHead[0] === foodSpot[0] && newHead[1] === foodSpot[1]) {
			hasEaten = true
		}
		snake = [newHead, ...snake.slice(0, hasEaten ? snake.length : snake.length - 1)]
		if (hasEaten) {
			foodSpot = randomFoodSpot(gridSize, snake)
			console.log(foodSpot)
			clearInterval(ticker)
			interval /= 1.1
			ticker = setInterval(tick, interval)
		}
	}

	let rows = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19];

	function handleKeydown(event) {
		const keyCode = event.keyCode
		switch (keyCode) {
			case 37:
				direction = "WEST"
				break;
			case 38:
				direction = "NORTH"
				break;
			case 39:
				direction = "EAST"
				break;
			case 40:
				direction = "SOUTH"
				break;
		}
	}

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
	<div class="grid">
		{#each rows as rowIndex}
			<Row {rowIndex} {snake} {foodSpot}/>
		{/each}
	</div>
</main>

<svelte:window on:keydown={handleKeydown}/>

<style>
	main {
		background-color: #ddd;
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	.grid {
		background-color: #fff;
	}
</style>
