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
let steps=180
let spheres = 4;

for (let p = 0; p<=spheres; p++){
    let dots = [];
    for (let i=0; i<=steps; i++){
        let [x,y,z] = random.onSphere(r, []);
        dots.push([x+cx,y+cy,z])
    }

    let points = [];

    while(dots.length >1){
        let start = dots.pop();
        dots.sort((a, b) => vec3.squaredDistance(b,start) - vec3.squaredDistance(a,start) );
        points.push(start)
    }
    
    let path = `M${points[0][0]},${points[0][1]}`;
    points.map(point =>{
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