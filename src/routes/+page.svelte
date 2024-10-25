<script lang="ts">
	import { ColorSpace, sRGB, HSL, parse, to } from 'colorjs.io/fn';
	import { Poline } from 'poline';

	let colors = '#4F405E\n#191b0e';
	$: anchorColors = colors.split('\n');
	let numPoints = 2;

	ColorSpace.register(sRGB);
	ColorSpace.register(HSL);
	$: anchorColorsHSL = anchorColors
		.map((color) => {
			try {
				const [hue, saturation, lightness] = to(parse(color), 'hsl').coords;
				return [hue, saturation / 100, lightness / 100] as [number, number, number];
			} catch (e) {}
		})
		.filter((color) => color !== undefined);

	$: poline = new Poline({
		anchorColors:
			anchorColorsHSL.length >= 2
				? anchorColorsHSL
				: [
						[0, 0, 0],
						[1, 1, 1],
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
		<span>Anchor Colors</span>
		<textarea bind:value={colors} />
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
