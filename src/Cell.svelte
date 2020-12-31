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
        height: min(min(30px, 5vh), 5vw);
        width: min(min(30px, 5vh), 5vw);
    }

    .isSnake {
        background-color: #1f3b06;
        border-radius: 15%;
    }

    .isFoodSpot {
        background-image: url("/mouse-svgrepo-com.svg");
        border-radius: 15%;
    }
</style>
