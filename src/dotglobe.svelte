<script>
import random from 'canvas-sketch-util/random';
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
    <path d={`M${cx},${cy}L${x},${y}`} opacity=0.2 stroke={random.pick(colors)}/>
    <circle cx={x} cy={y} r={(r/15) * (z/r)} fill={random.pick(colors)}/>
{/each}
</g>