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
  { id: 1, name: "loc_1", x: 850, y: 655 },
  {  id: 2,name: "loc_2", x: 200, y: 500},
  {  id: 3,name: "loc_3", x: 760, y: 550},
  {  id: 4,name: "loc_4", x:500, y: 600},
  { id: 5, name: "loc_5", x: 294, y: 286 },
];

console.log(locations)

onMount(() => {


	let wrapperBounds = document.querySelector('.map-wrapper').getBoundingClientRect();
	let graphicBounds = document.querySelector('.map-graphic').getBoundingClientRect();

  let wrapperWidth = wrapperBounds.width;
  let wrapperHeight = wrapperBounds.height;
  let wrapperLocation = [wrapperBounds.top, wrapperBounds.left, wrapperBounds.bottom, wrapperBounds.right]


  let graphicWidth = graphicBounds.width;
  let graphicHeight = graphicBounds.height;
  let graphicOffset = [graphicBounds.x, graphicBounds.y]



locations.forEach(function(marker_index){
	console.log(marker_index);
	// document.getElementById("mark-location").style.backgroundImage ="url('//news.northeastern.edu/interactive/2021/10/commencement-2021/images/icon_"+ marker_index.id + ".png')";

	 // let el = document.getElementById("mark-location");
	 // // console.log(el)
	 // el.className = 'marker';
	 // console.log(el)
	 // let el = document.createElement('div');
		//    el.className = 'marker';
	 //
	 // el.style.backgroundImage = "url('https://img.icons8.com/ios/50/000000/building.png')";

})


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
	// let mapPoints = document.querySelectorAll('.map-point');
 let mapPoints = document.querySelectorAll('.foreign-marker');
	let mapPointBounds = mapPoints[index - 1].getBoundingClientRect();

	let graphicElement = document.getElementById('graphic');

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
	width: 100vw;
    height: 600px;
	/* width: 1024px;
	height: 768px; */
	/* width: 600px;
	height: 600px; */
	/* border: red solid; */
	/* width: 1024px;
	height:768px; */
}

.map-graphic{
	position:absolute;
	top: 0;
left: 0;
	width: 1024px;
	height: 800px;
	transition: top 2s ease 0s, left 2s ease 0s
}

image.commencement-map{
	width: 1024px;
	height:768px;

}


.marker{
	/* height: 50px;
width: 50px;
background-color: #000;
border-radius: 50%;
display: inline-block;
z-index: 1; */
/* position:absolute; */
/* position: absolute; */
/* margin: 200px 50px */

/* background-image: url("https://img.icons8.com/ios/50/000000/building.png"); */
   background-repeat: no-repeat;
     background-position: center; /* Center the image */
background-size: 40px;

/* background-image: url("//news.northeastern.edu/interactive/2021/05/commencement-map-2021/photos/1.jpg"); */
/* <img src="https://img.icons8.com/ios/50/000000/building.png"/> */
/* transform: translate(-50%, -50%) translate(-68px, 191px) rotateX(0deg) rotateZ(0deg); */
    /* background-size: cover; */
    border: 4px solid black;
    width: 70px;
    height: 70px;
    border-radius: 50%;
    cursor: pointer;
    position: absolute;
    /* top: 0; */
	top: 0;
 	left: 0;
	/* top: 25%;
	left: 25%; */
    will-change: transform;
    z-index: 1;
}

.marker::after {
    content: " ";
    position: absolute;
    bottom: -35%;
    left: 50%;
    margin-left: -10px;
    border-width: 10px;
    border-style: solid;
    border-color: black transparent transparent transparent;
	z-index: 1;

}

/* .marker:hover{

  transform: scale(1.5);
} */

.map-point{
	fill: blue;
	r: 20;
	stroke: black;
	stroke-width: 2;
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

<div class = "category-title">Commencement 2020 Map</div>

<!-- <div bind:this={el} class="chart"></div> -->

<!-- <img src =  {src_images} alt = "commencement-map-2021" class = "commencement-map"> -->
<div class = "map-wrapper">
<!-- {#each locations as location}

<g class = "marker" style="position:absolute; left:{location.x}px; top:{location.y}px;"></g>
{/each} -->
<svg class = "map-graphic" id = "graphic"
  xmlns="http://www.w3.org/2000/svg">

 <image href={src_images}  class = "commencement-map"/>
 {#each locations as location}

 <!-- <polygon points="5,5 195,10 185,185 10,195" /> -->

 <!-- Common use case: embed HTML text into SVG -->
 <foreignObject x="{location.x}" y="{location.y}" width="80" height="80" class = "foreign-marker">
   <!--
	 In the context of SVG embedded in an HTML document, the XHTML
	 namespace could be omitted, but it is mandatory in the
	 context of an SVG document
   -->
   <!-- <div class = "marker-circle" xmlns="http://www.w3.org/1999/xhtml">
	 Lorem ipsum dolor sit amet, consectetur adipiscing elit.
	 Sed mollis mollis mi ut ultricies. Nullam magna ipsum,
	 porta vel dui convallis, rutrum imperdiet eros. Aliquam
	 erat volutpat.
   </div> -->

   <!-- <div class = "marker-cicle"> -->
   <!-- <span class = "marker" id = "mark-location" style="background-image: url('//news.northeastern.edu/interactive/2021/10/commencement-2021/images/icon_1.png')" xmlns="http://www.w3.org/1999/xhtml"></span> -->
   <span class = "marker" id = "mark-location" style="background-image:url('//news.northeastern.edu/interactive/2021/10/commencement-2021/images/icon_{location.id}.png" xmlns="http://www.w3.org/1999/xhtml"></span>

   <!-- </div> -->
   <!-- <span class = "marker-after"  xmlns="http://www.w3.org/1999/xhtml"></span> -->

 </foreignObject>
  <!-- <span class = "marker"></span> -->


	<!-- <g class = "marker" style="position:absolute; left:{location.x}px; top:{location.y}px;"></g> -->

  <!-- <circle cx="{location.x}" cy="{location.y}" r="20" class="map-point" /> -->
  {/each}
  <!-- <circle cx="500" cy="250" r="20" stroke="black" stroke-width="2" fill="purple" />
  <circle cx="1000" cy="568" r="20" stroke="black" stroke-width="2" fill="purple" />
  <circle cx="212" cy="294" r="20" stroke="black" stroke-width="2" fill="purple" /> -->

</svg>

</div>
<!-- <div class = "location-name" on:click={() => navTo()}>location 2</div> -->

<div class = "content-wrapper">
<div class = "wrapper-hide"></div>
</div>
{#each locations as location}

<div class = "location-name" on:click={() => navTo(location.id)}>location {location.id}</div>
<!-- <div class = "location-name" on:click={() => navTo(2)}>location 2</div> -->
{/each}
<!-- <div class = "location-name" on:click={() => navTo(3)}>location 3</div> -->
<!-- <div class = "location-name" on:click={() => navTo(5)}>location 5</div> -->


<!-- </main> -->
