<script>
import vec2 from "gl-vec2"
import geometry from 'canvas-sketch-util/geometry';
import random from 'canvas-sketch-util/random';
import {intersetCircleCircle} from './indersect-circle-circle.js'


export let seed=10
random.setSeed(seed)

export let colors = ["#8AACA6","#861F16","#FED340","#ED6A66"]	
export let cx=0
export let cy=0
export let r=20
export let lines = random.rangeFloor(2,12) ;
export let thickness = (r*2.5)/(lines);


let center = [parseInt(cx),parseInt(cy)];


//
let hangerPath  = `M${cx},0V${cy- r -8}`


// Path for Connector

let hangerR = r/6;

let pathOffset= hangerR/4
let hangerCenter = [parseInt(cx),parseInt(cy)- r - pathOffset ];


let intersection = intersetCircleCircle(center, parseInt(r)+pathOffset, hangerCenter, hangerR);

let [pi1, pi2] = intersection;
let connectorPath =`M${pi1}` 
	connectorPath += `A ${hangerR} ${hangerR} 0 0 0 ${pi2[0]} ${pi2[1]}`
	connectorPath += `A ${parseInt(r)} ${parseInt(r)} 0 0 1 ${pi1[0]} ${pi1[1]}`


	
let paths = []

let fills = [];
	
for (let i = 0; i< lines; i++){
	let a = [-r*2,0];
	let b = [r*2,0];
	
	let offset = vec2.lerp([],[0,-r],[0,r] , i/(lines-1));
	vec2.add(offset, offset, center)	
	vec2.add(offset,offset,[0,-thickness/2])

	let angle = random.range(Math.PI/-lines, Math.PI/lines);
 	
	vec2.rotate(a,a,angle)
	vec2.rotate(b,b,angle)
	vec2.add(a,a,offset)
	vec2.add(b,b,offset)
	let line = []
	geometry.clipLineToCircle(a, b, center, r, line)
	vec2.add(a,a,[0,thickness])
	vec2.add(b,b,[0,thickness])	
	let line2 = []
	let collide = geometry.clipLineToCircle(a, b, center, r, line2)
	let path = ""
	
	if(line.length >1){
		let[p1,p2] = line;
		path += `M ${p1}L${p2}`
		if(line2.length >1){
			let[p3,p4] = line;
			path += `A ${r} ${r} 0 0 0 ${line2[1][0]} ${line2[1][1]}`
			path += `L ${line2[1]}L${line2[0]}`
			path += `A ${r} ${r} 0 0 0 ${line[0][0]} ${line[0][1]}`
		} else{
			path += `A ${r} ${r} 0 0 0 ${line[0][0]} ${line[0][1]}`
		}
	} else{
			if(line2.length >1){
				path += `M ${line2[0]}L${line2[1]}`
				path += `A ${r} ${r} 0 0 1 ${line2[0][0]} ${line2[0][1]}`
			}	
	}

	if(path != "") {
		paths.push(path)	
		fills.push(random.pick(colors))
	}
}


</script>
<g>

<path d="{hangerPath}" stroke="{fills[0]}"  stroke-dasharray="0.01 10" stroke-linecap="round" stroke-width="4"/>
<path d="{connectorPath}" fill="{fills[0]}"/>


{#each paths as d, i}
	<path {d} fill="{fills[i]}" opacity=0.8></path>
{/each}
</g>

<style>

</style>
