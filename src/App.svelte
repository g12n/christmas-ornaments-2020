<script>
	import Globe from './globe.svelte';
	let width=360; 
	let height = 640;	
	let points = []

	for (let i = 1; i <= 3; i++){
		points.push([Math.random()* width, Math.random()* height, Math.random()* width/4 + width/12])
	}




</script>

<svg viewBox="0 0 {width} {height}">
	<rect fill="#F0EEE6" {width} {height}  />

	{#each points as [cx,cy,r], i}
		<Globe {cx} {cy} {r} seed="{Math.random()}"/>
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