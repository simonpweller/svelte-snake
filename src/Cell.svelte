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
        background-color: #dddddd;
        border: 1px solid #666666;
    }

    .isSnake {
        background-color: chartreuse;
    }

    .isFoodSpot {
        background-color: lightpink;
    }
</style>
