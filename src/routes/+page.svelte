<script lang="ts">
	import { ColorSpace, sRGB, HSL, parse, to } from 'colorjs.io/fn';
	import { Poline } from 'poline';

	let anchorColor1 = '#4F405E';
	let anchorColor2 = '#191b0e';
	let numPoints = 2;

	ColorSpace.register(sRGB);
	ColorSpace.register(HSL);
	$: anchorColor1HSL = to(parse(anchorColor1), 'hsl').coords;
	$: anchorColor2HSL = to(parse(anchorColor2), 'hsl').coords;

	$: poline = new Poline({
		anchorColors: [
			[anchorColor1HSL[0], anchorColor1HSL[1] / 100, anchorColor1HSL[2] / 100],
			[anchorColor2HSL[0], anchorColor2HSL[1] / 100, anchorColor2HSL[2] / 100],
		],
		numPoints,
	});
</script>

<div class="colors">
	{#each poline.colorsCSS as color}
		<div class="color" style:background-color={color} />
	{/each}
</div>
<div class="inputs">
	<label>
		<span>Anchor Color 1</span>
		<input type="color" bind:value={anchorColor1} />
	</label>
	<label>
		<span>Anchor Color 2</span>
		<input type="color" bind:value={anchorColor2} />
	</label>
	<label>
		<span>Number of Points</span>
		<input type="number" min="1" bind:value={numPoints} />
	</label>
</div>

<style>
	.inputs {
		margin-top: 1rem;
		margin-bottom: 1rem;
	}

	.colors {
		display: flex;
	}

	.color {
		width: 100px;
		height: 100px;
	}
</style>
