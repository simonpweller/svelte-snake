<script>
    export let rowIndex;
    export let snake;
	import Cell from './Cell.svelte';
	let cols = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19];
</script>
<div>
    {#each cols as colIndex}
        <Cell {rowIndex} {colIndex} {snake}/>
    {/each}
</div>

<style>
    div {
        display: flex;
    }
</style>
