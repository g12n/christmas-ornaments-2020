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



let paths = [];

   let start = [cx,cy-r,0];

let spheres = 6; 
let steps=180

for (let p = 1; p<= spheres; p++){
   
    let radius =  mapRange(p, 1, spheres, r, r/spheres, true)

    
    start = [cx,cy-radius,0];

    let dots = [];
    for (let i=0; i<=steps; i++){
        let [x,y,z] = random.onSphere(radius, []);
        dots.push([x+cx,y+cy,z])
    }
 
    dots.sort((a, b) => vec3.squaredDistance(a,start) - vec3.squaredDistance(b,start) );
    let path = `M${start[0]},${start[1]}`;
    dots.map(point =>{
        path += `L${point[0]},${point[1]}`;
    })
    paths.push(path)
}


</script>

<g>
{#each paths as path, i}
<path d={path} stroke={random.pick(colors)} opacity="0.5" fill="none"/>
{/each}



</g>