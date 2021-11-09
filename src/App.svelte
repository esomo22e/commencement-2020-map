<script>
	import { onMount } from 'svelte';

	//Maps
	import MapUSA from './charts/map-usa.svelte'

	//Graphic Components
	import GraphicTitle from './components/GraphicTitle.svelte'
	import GraphicFooter from './components/GraphicFooter.svelte'

	//Helpers
	import * as colors from './helpers/colors.js'

	//Dataset
	import * as turnout from '../public/datasets/turnout.json'
	import * as applemaps from '../public/datasets/applemaps.json'
	import * as cannabislaws from '../public/datasets/cannabislaws.json'
	import * as waffledata from '../public/datasets/waffledata.json'
	import * as testData from '../public/datasets/test.json'

	import * as d3 from 'd3';
	var data = [30, 86, 168, 281, 303, 365];

	let el;


	export let width = Math.min(

		document.getElementById('svelte-commencement-2021').getBoundingClientRect().width,
		1100
	);

	console.log(waffledata.default[0])

	export let height = 350;

	function getorientation(w) {
		if (w > 750) {
			return "vertical"
		} else {
			return "horizontal"
		}
	}

	let src_images = "http://news.northeastern.edu/interactive/2021/10/commencement-2021/images/map_northeastern.svg";

	// export let lat;
	// export let long;

	const markerLocation = [];


export const locations = [
  { id: 1, name: "loc_1", x: 850, y: 707 },
  {  id: 2,name: "loc_2", x: 200, y: 500},
  {  id: 3,name: "loc_3", x: 300, y: 550},
  {  id: 4,name: "loc_4", x:120, y: 700},
  { id: 5, name: "loc_5", x: 294, y: 286 },
];



onMount(() => {

// console.log(graphicElement)

// function getViewport(){
	let wrapperBounds = document.querySelector('.map-wrapper').getBoundingClientRect();
	// console.log(wrapperBounds);
	let graphicBounds = document.querySelector('.map-graphic').getBoundingClientRect();
   // console.log(graphicBounds)

  let wrapperWidth = wrapperBounds.width;
  let wrapperHeight = wrapperBounds.height;
  let wrapperLocation = [wrapperBounds.top, wrapperBounds.left, wrapperBounds.bottom, wrapperBounds.right]
  // console.log(wrapperWidth)
  // console.log(wrapperHeight)
  // console.log(wrapperLocation[0])

  let graphicWidth = graphicBounds.width;
  let graphicHeight = graphicBounds.height;
  let graphicOffset = [graphicBounds.x, graphicBounds.y]
  // console.log(graphicWidth)
  // console.log(graphicHeight)
  // console.log("x:" + graphicOffset[0])


// }
// getViewport();


});

function navTo(index) {
	// alert('clicked')
	console.log("clicked" + (index-1))
	console.log(locations);
	console.log(locations.length)

	let location = locations[index -1]
	console.log(location)
	let graphicBounds = document.querySelector('.map-graphic').getBoundingClientRect();
	let wrapperBounds = document.querySelector('.map-wrapper').getBoundingClientRect();

	console.log("x graphics bound:" +graphicBounds.x)
	console.log("y graphics bound:" +graphicBounds.y)

	console.log("x wrapper height:" +graphicBounds.x)
	console.log("y wrapper width:" +graphicBounds.y)

	console.log(location.y)
	console.log(location.x)
	let mapPoints = document.querySelectorAll('.map-point');

	let mapPointBounds = mapPoints[index - 1].getBoundingClientRect();

	let graphicElement = document.getElementById('graphic');
	// graphicElement.style.top = location.y + graphicBounds.y+ "px";
	// graphicElement.style.left = location.x + graphicBounds.x/2 + "px";
	// graphicElement.style.top =  (wrapperBounds.height/4 +graphicBounds.height/4-location.y) + "px";
	// graphicElement.style.left = (wrapperBounds.width/4 +graphicBounds.width/4  - location.x) + "px";
	graphicElement.style.top =  (wrapperBounds.height/2 -mapPointBounds.height/2-location.y) + "px";
	graphicElement.style.left = (wrapperBounds.width/2 -mapPointBounds.width/2  - location.x) + "px";
	console.log(graphicElement.style.top);
	console.log(graphicElement.style.left);

	// locations[3];
	// locations = locations[index-1];
}
</script>

<style>

.category-title{
	font-size: 40px;
	font-weight: 600;
	text-align: center;
}

main{
	/* margin: 0 auto;
	display: block;
	margin-left: 25%;
	margin-right: 25%; */
}

.map-wrapper{
	overflow: hidden;
	position: relative;
	width: 600px;
	height: 600px;
	border: red solid;
	/* width: 1024px;
	height:768px; */
}

.map-graphic{
	position:absolute;
	top: 0;
left: 0;
	width: 1024px;
	height: 768px;
	transition: top 2s ease 0s, left 2s ease 0s
}

image.commencement-map{
	width: 1024px;
	height:768px;

}



/* .chart :global(div) {
		font: 10px sans-serif;
		background-color: steelblue;
		text-align: right;
		padding: 3px;
		margin: 1px;
		color: white;
	} */
	/* svg {
	  display: block;
	  width: 100%;
	} */
</style>
<!-- <main> -->

<div class = "category-title">Commencement 2021 Map</div>

<!-- <div bind:this={el} class="chart"></div> -->

<!-- <img src =  {src_images} alt = "commencement-map-2021" class = "commencement-map"> -->
<div class = "map-wrapper">
<svg class = "map-graphic" id = "graphic"
  xmlns="http://www.w3.org/2000/svg">
  <image href={src_images}  class = "commencement-map"/>
    {#each locations as location}
  <circle cx="{location.x}" cy="{location.y}" r="20" stroke="black" stroke-width="2" fill="purple" class="map-point" />

  {/each}
  <!-- <circle cx="500" cy="250" r="20" stroke="black" stroke-width="2" fill="purple" />
  <circle cx="1000" cy="568" r="20" stroke="black" stroke-width="2" fill="purple" />
  <circle cx="212" cy="294" r="20" stroke="black" stroke-width="2" fill="purple" /> -->

</svg>

</div>
<!-- <div class = "location-name" on:click={() => navTo()}>location 2</div> -->

{#each locations as location}

<div class = "location-name" on:click={() => navTo(location.id)}>location {location.id}</div>
<!-- <div class = "location-name" on:click={() => navTo(2)}>location 2</div> -->
{/each}
<!-- <div class = "location-name" on:click={() => navTo(3)}>location 3</div> -->
<!-- <div class = "location-name" on:click={() => navTo(5)}>location 5</div> -->


<!-- </main> -->
