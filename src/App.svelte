<script>
	import Globe from './globe.svelte';
	import FastPoissonDiskSampling from "fast-2d-poisson-disk-sampling"
	import * as tome from 'chromotome';
	import random from 'canvas-sketch-util/random';

	let width=360; 
	let height = 640;	
	
	
	let palette = {
		background: "#F0EEE6",
		colors:["#8AACA6","#861F16","#FED340","#ED6A66"],
		name: "ornaments bright",
		size: 6,
		stroke: "#fff"
	}	

	palette = tome.get(); // tome.getRandom() also works.


	let points = []
/*
	for (let i = 1; i <= 3; i++){
		points.push([Math.random()* width, Math.random()* height, Math.random()* width/4 + width/12])
	}
*/
var p = new FastPoissonDiskSampling({
    shape: [width*0.75, height*0.75],
    radius: width*0.65,
    tries: 20
});
var pts = p.fill();

pts.map(point=>{
	console.log(point)
	points.push([point[0]+width*0.125, point[1]+height*0.125,random.range(width/6,width/4 )])
}) 



</script>

<svg viewBox="0 0 {width} {height}">
<title>Christmas-Ornaments 2020 by Michael Gehrmann – {palette.name}</title>
	<rect fill="{palette.background}" {width} {height}  />
	{#each points as [cx,cy,r], i}
		<Globe {cx} {cy} {r} seed="{Math.random()}" colors={palette.colors} stroke="{palette.stroke}"/>
	{/each}
	<!--
	<Globe cx=220 cy=200 r=90 seed="16"/>
	<Globe cx=190 cy=440 r=40 seed="17"/>
	<Globe cx=120 cy=340 r=60 seed="9"/>
	-->
</svg>


<style>

svg{max-width: 100vmin;
	max-height: 100vmin;}
</style>