<script lang="ts">
	import { ColorSpace, sRGB, HSL, parse, to } from 'colorjs.io/fn';
	import { Poline } from 'poline';

	let anchorColors = ['#4F405E', '#191b0e'];
	let numPoints = 2;

	ColorSpace.register(sRGB);
	ColorSpace.register(HSL);
	$: anchorColorsHSL = anchorColors.map((color) => {
		const [hue, saturation, lightness] = to(parse(color), 'hsl').coords;
		return [hue, saturation / 100, lightness / 100] as [number, number, number];
	});

	$: poline = new Poline({
		anchorColors: anchorColorsHSL,
		numPoints,
	});
</script>

<div class="colors">
	{#each poline.colorsCSS as color}
		<div class="color" style:background-color={color} />
	{/each}
</div>
<div class="inputs">
	{#each anchorColors as color, i}
		<label>
			<span>Anchor Color {i + 1}</span>
			<input type="color" bind:value={color} />
		</label>
		<button
			disabled={anchorColors.length === 2}
			on:click={() => (anchorColors = anchorColors.filter((_, j) => j !== i))}>Remove</button
		>
	{/each}
	<button on:click={() => (anchorColors = [...anchorColors, '#191b0e'])}>Add Anchor</button>
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
