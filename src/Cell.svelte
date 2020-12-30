<script>
    export let rowIndex;
	export let colIndex;
    export let snake;

    $: isSnake = snake.some(([row, col]) => row === rowIndex && col === colIndex)
</script>

<div class:isSnake></div>

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
</style>
