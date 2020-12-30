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

let paths = [];
let offset = random.range(0.5, 1)

dots.map(point =>{
    let start = vec3.lerp([],center, point, offset);
    paths.push(`M${start[0]},${start[1]}L${point[0]},${point[1]}`)

})

</script>

<g>


{#each paths as path, i}
    <path d={path} opacity={mapRange(dots[i][2], -r, r, 0.2, 1, true)} stroke={random.pick(colors)}/>
{/each}



</g>