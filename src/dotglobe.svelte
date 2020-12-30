<script>
import random from 'canvas-sketch-util/random';
import {mapRange} from 'canvas-sketch-util/math'






import vec3 from "gl-vec3"


export let cx=0
export let cy=0
export let r=20
export let seed=10
export let colors = ["#8AACA6","#861F16","#FED340","#ED6A66"]	

random.setSeed(seed)

let center = [cx,cy,0];
let dots = [];

for (let i=0; i<=180; i++){
    let [x,y,z] = random.onSphere(r, []);
    dots.push([x+cx,y+cy,z])
}

dots.sort((a, b) => a[2] - b[2] );

</script>

<g>
{#each dots as [x,y,z], i}
    {#if z <= 0}
    <circle cx={x} cy={y} r={mapRange(z, -r, r, 1, 4, true)}  opacity={mapRange(z, -r, r, 0.2, 1, true)} fill={random.pick(colors)}/>
    {/if}
{/each}

{#each dots as [x,y,z], i}
    <path d={`M${cx},${cy}L${x},${y}`} opacity={mapRange(z, -r, r, 0, 0.2, true)} stroke={random.pick(colors)}/>
{/each}

{#each dots as [x,y,z], i}
   {#if z >= 0}
    <circle cx={x} cy={y} r={mapRange(z, -r, r, 1, r/20, true)} opacity={mapRange(z, -r, r, 0.2, 1, true)} fill={random.pick(colors)}/>
    {/if}
{/each}


</g>