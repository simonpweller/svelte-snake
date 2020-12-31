<script>
    export let rowIndex;
	export let colIndex;
    export let snake;
    export let foodSpot;

    $: isSnake = snake.some(([row, col]) => row === rowIndex && col === colIndex)
    $: isFoodSpot = foodSpot[0] === rowIndex && foodSpot[1] === colIndex
</script>

<div class:isSnake class:isFoodSpot></div>

<style>
    div {
        height: 30px;
        width: 30px;
    }

    .isSnake {
        background-color: #1f3b06;
        border-radius: 15%;
    }

    .isFoodSpot {
        background-color: lightpink;
        border-radius: 15%;
    }
</style>
