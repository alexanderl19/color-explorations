<script lang="ts">
	import { Poline } from 'poline';

	const poline = new Poline({
		anchorColors: [
			[309, 0.72, 0.8],
			[67, 0.32, 0.08],
		],
		numPoints: 2,
	});
</script>

<div class="colors">
	{#each poline.colorsCSS as color}
		<div class="color" style:background-color={color} />
	{/each}
</div>

<style>
	.colors {
		display: flex;
	}

	.color {
		width: 100px;
		height: 100px;
	}
</style>
