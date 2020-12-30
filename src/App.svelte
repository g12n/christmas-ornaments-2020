<script>
	import Globe from './globe.svelte';
	import Dotglobe from './dotglobe.svelte';
	import Sparkler from './sparkler.svelte';
	import sphereA from './sphere-a.svelte';
	import sphereB from './sphere-b.svelte';



	let ornaments = [ Globe, Dotglobe, Sparkler,sphereA,  sphereB ]

	import FastPoissonDiskSampling from "fast-2d-poisson-disk-sampling"
	import * as tome from 'chromotome';
	import random from 'canvas-sketch-util/random';
	import {saveSvgAsPng} from  'save-svg-as-png'
	import { saveAs } from 'file-saver';


	let width=360; 
	let height = 640;

	let vmin = Math.min(width,height);

	let maxr = vmin*0.55;
	let padding =  vmin* 0.33;
	let svg;
	
	let palette = {
		background: "#F0EEE6",
		colors:["#8AACA6","#861F16","#FED340","#ED6A66"],
		name: "ornaments bright",
		size: 6,
		stroke: "#fff"
	}


	palette = tome.get(); // tome.getRandom() also works.
	let points = [[250,400,50],[140,200,80]]

	$: title = `Christmas-Ornaments 2020 by Michael Gehrmann – ${points.length} Ornaments Color Scheme ${palette.name}`


let updatePoints = ()=>{
	points = points.slice(points.length);

	var p = new FastPoissonDiskSampling({
    	shape: [width-padding*2, height-padding*2],
    	radius: maxr,
    	tries: 10
	});

	var pts = p.fill();
	pts.map(point=>{
		let x = parseInt( point[0]+padding)
		let y = parseInt( point[1]+padding )
		let r = parseInt(random.rangeFloor(maxr/3,maxr/2 ))
		points.push([x, y,r])
	})
}

updatePoints()

let updatePalette = () => {
	palette = tome.get()
}

let saveDesginAsPng = () =>{
	saveSvgAsPng(svg, `ornament-${palette.name}.png`, {scale: 3});
}

let saveDesginAsSvg = () =>{
	var blob = new Blob([svg.outerHTML], {type: "image/svg+xml;charset=utf-8"});
	saveAs(blob, `ornament-${palette.name}.svg`);
}
</script>

<section class="preview">
	<svg viewBox="0 0 {width} {height}" bind:this={svg}  xmlns="http://www.w3.org/2000/svg">
		<title>{title}</title>
		<rect id="background" fill="{palette.background}" {width} {height}  />
		{#each points as [cx,cy,r] (`${cx}-${cy}-${r}-${palette.name}`)}
			<svelte:component this={random.pick(ornaments)} {cx} {cy} {r} seed="{Math.random()}" colors={palette.colors}/>
		{/each}
	</svg>
</section>

<section class="actions">
<button class="layout" title="Update drawing" on:click={updatePoints} > Update Drawing </button>
<button class="color"  title="update colors" on:click={updatePalette} > Update Colors </button>
<button class="png" title="download as png" on:click={saveDesginAsPng} > Download PNG </button>
<button class="svg "title="download as svg" on:click={saveDesginAsSvg} > Download SVG </button>

</section>
<style>
:global(body) {
	display: grid;
	grid-template: "body";
	grid-template-rows: 1fr auto;
	margin: 0;
	padding: 0px;
	height: 100vh;
	background: #000
}

.preview{
	grid-area: body;
}
.actions{
	grid-area: body;

	display: grid;	
	grid-template: "layout . color" ". . ." "png . svg";
	grid-template-rows: auto 1fr auto;
	grid-template-columns: auto 1fr auto;
	padding: 1rem;
	grid-gap: 1rem;
}

.layout{ grid-area: layout }
.color{ grid-area: color }
.png{grid-area: png}
.svg{grid-area: svg}

svg{

width: 100%; height: 100%; max-height: 100vh;
	}
</style>