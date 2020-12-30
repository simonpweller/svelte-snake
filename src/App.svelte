<script>
	import Row from './Row.svelte';
	const gridSize = 20

	let snake = [[10, 10], [10, 11], [10, 12], [10, 13]]
	let direction = "NORTH"
	setInterval(() => {
		let [headRow, headCol] = snake[0]
		let newHead;
		switch(direction) {
			case 'EAST':
				newHead = [headRow, (headCol + 1 === gridSize) ? 0 : headCol + 1]
				break;
			case 'SOUTH':
				newHead = [(headRow + 1 === gridSize) ? 0 : headRow + 1, headCol]
				break;
			case 'WEST':
				newHead = [headRow, (headCol - 1 < 0) ? gridSize -1 : headCol - 1]
				break;
			case 'NORTH':
				newHead = [(headRow - 1 < 0) ? gridSize - 1 : headRow - 1, headCol]
				break;
		}
		snake = [newHead, ...snake.slice(0, snake.length -1)]


	}, 500)
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
</script>

<main>
	<div class="grid">
		{#each rows as rowIndex}
			<Row {rowIndex} {snake}/>
		{/each}
	</div>
</main>

<svelte:window on:keydown={handleKeydown}/>

<style>
	main {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}
</style>
