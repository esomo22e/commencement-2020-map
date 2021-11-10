<script>
	import { onMount } from 'svelte';

	//Maps
	import MapUSA from './charts/map-usa.svelte'

	//Graphic Components
	import GraphicTitle from './components/GraphicTitle.svelte'
	import GraphicFooter from './components/GraphicFooter.svelte'

	//Helpers

	//Dataset
	import * as turnout from '../public/datasets/turnout.json'
	import * as applemaps from '../public/datasets/applemaps.json'
	import * as cannabislaws from '../public/datasets/cannabislaws.json'
	import * as waffledata from '../public/datasets/waffledata.json'
	import * as testData from '../public/datasets/test.json'


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
  { id: 1,
	  name: 'Larger than Life 2020',
	  description: 'Come by Centennial Common to take your photo with the 20’ high ‘2020’ to mark this moment!',
  x: 850,
  y: 655
},
  {  id: 2,
	  name: 'Easy Being Green',
		description: 'The Sculpture Garden holds a peaceful spot for a very green photo backdrop for photos of you and your loved ones.',
	  x: 200,
	  y: 500
  },
  { id: 3,
	  name: 'Blowing in the Wind',
	  description: 'Krentzman Quad holds a great collection of pinwheels for fun photos!',
	  x: 760,
	  y: 550
  },
  {  id: 4,
	  name: 'You’re So Fancy!',
    description: 'Bright red sequins as a flashy background for you and friends at base of walking bridge between Snell and Egan!',
	  x:500,
	  y: 600
  },
  { id: 5,
	  name: 'N2020',
    description: 'Doesn’t matter what direction you’re headed in, stop by Snell Quad for a great N2020 photo op!',
	   x: 294,
	   y: 286
   },
];



onMount(() => {

	let mapWrapper = document.querySelector('.map-wrapper');


	let locationInfo = document.querySelector('.nu-map-location-info');
	let locationInfoTitle = document.querySelector('.nu-map-location-title');
	let locationInfoText = document.querySelector('.nu-map-location-text');
	let locationInfoToggle = document.querySelector('.nu-map-location-info__toggle');
	let prevButton = document.querySelector('.nu-map-button--prev');
	let nextButton = document.querySelector('.nu-map-button--next');
	// const mapWrapper = document.querySelector('.map-wrapper');
	let canvasElement = document.querySelector('#nu-map-effects');
	// const canvasPath = canvasElement.getContext('2d');
	let minimumParticles = 5;
	let colors = ['#badb00', '#00cfb5', '#ff854f', '#ffbf3d', '#006eb5'];
	let particleInterval;
	let _particles = [];
	let activeLocation = 0;

	let wrapperBounds = document.querySelector('.map-wrapper').getBoundingClientRect();
	let graphicBounds = document.querySelector('.map-graphic').getBoundingClientRect();

  let wrapperWidth = wrapperBounds.width;
  let wrapperHeight = wrapperBounds.height;
  let wrapperLocation = [wrapperBounds.top, wrapperBounds.left, wrapperBounds.bottom, wrapperBounds.right]


  let graphicWidth = graphicBounds.width;
  let graphicHeight = graphicBounds.height;
  let graphicOffset = [graphicBounds.x, graphicBounds.y]


// let tl = gsap.timeline({ease: Power2.in});



    init();

    function init() {
        // canvasElement.height = mapWrapper.clientHeight;
        // canvasElement.width = mapWrapper.clientWidth;

        activateLocation(activeLocation);

        nextButton.addEventListener('click', () => {
            activateLocation(activeLocation + 1);
        });

        prevButton.addEventListener('click', () => {
            activateLocation(activeLocation - 1);
        });

        document.addEventListener('keyup', (event) => {
            switch (event.key) {
                case "ArrowLeft":
                    activateLocation(activeLocation - 1);
                    break;
                case "ArrowRight":
                    activateLocation(activeLocation + 1);
                    break;
            }
        })

        locationInfoToggle.addEventListener('click', () => {
            if (locationInfo.classList.contains('is-active')) {
                locationInfo.classList.remove('is-active');
            } else {
                locationInfo.classList.add('is-active');
            }
        });

        locationInfoTitle.addEventListener('click', () => {
            if (locationInfo.classList.contains('is-active')) {
                locationInfo.classList.remove('is-active');
            } else {
                locationInfo.classList.add('is-active');
            }
        });
    }

	function activateLocation(index) {

		if(index in locations){
			console.log(locations[index].id)

		}


		if (locations.length <= index) {
			index = 0;
		}
		if (index < 0) {
			index = locations.length - 1;
		}

		// TODO: Add navTo(index) call here.
		navTo(locations[index].id);
		activeLocation = index;
		let location = locations[index];
		locationInfoTitle.innerHTML = location.name;
		locationInfoText.innerHTML = location.description;
	}


});

function navTo(index) {


	let location = locations[index -1]
	let graphicBounds = document.querySelector('.map-graphic').getBoundingClientRect();
	let wrapperBounds = document.querySelector('.map-wrapper').getBoundingClientRect();


 let mapPoints = document.querySelectorAll('.foreign-marker');
	let mapPointBounds = mapPoints[index - 1].getBoundingClientRect();

	let graphicElement = document.getElementById('graphic');

	graphicElement.style.top =  (wrapperBounds.height/2 -mapPointBounds.height/2-location.y) + "px";
	graphicElement.style.left = (wrapperBounds.width/2 -mapPointBounds.width/2  - location.x) + "px";

}




</script>

<style>

:root {
  --nu-map--color--transparent-gray: rgba(211, 203, 203, 0.92);
  --nu-map--color--lighter-gray: #F8F8F9;

  --nu-map--max-width: 1024px;
  --nu-map--height: 706px;

  --nu-map-location-info--height: 170px;
  --nu-map-location-info--width: 300px;
  --nu-map-location--header--height: 42px;

  --nu-map-button--size: 52px;

  --global--color-red: #D41B2C;
  --global--color-black: #000;
  --global--color-white: #fff;

  --global--font-size-s: 1rem; /* ~16px */
  --global--font-small: akkurat, sans-serif;
  --global--font-display-impact: trim-poster, sans-serif;
    --global--font-weight-ultra-black: 950;
}


.nu-map-navigation-wrapper {
  z-index: 1;
  position: absolute;
  top: 0;
  right: 0;
  bottom: var(--nu-map-location-info--height);
  left: 0;
}

.nu-map-navigation {
  background: var(--global--color-black);
  justify-content: space-between;
  right: 0;
  box-sizing: border-box;
  display: flex;
  margin: 0;
  padding: 0;
  position: absolute;
  top: 0;
  width: var(--nu-map-navigation--width);
}

.nu-map-button {
  align-items: center;
  background-color: transparent;
  border-radius: 50%;
  border: none;
  color: var(--global--color-red);
  cursor: pointer;
  display: flex;
  flex: 1 1 auto;
  font-size: var(--nu-map-button-icon--size);
  height: var(--nu-map-button--size);
  justify-content: center;
  line-height: 1;
  right: 0;
  text-align: center;
  user-select: none;
  width: var(--nu-map-button--size);
  z-index: 3;
  -moz-user-select: none;
  -ms-user-select: none;
  -webkit-text-fill-color: var(--global--color-red);
  -webkit-text-stroke-color: var(--global--color-white);
  -webkit-text-stroke-width: 2px;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
}

.nu-map-button--prev {
  transform: rotateY(-180deg);
}

.nu-map-location-info {
  font-family: var(--global--font-small);
  font-size: var(--global--font-size-s);
  box-sizing: border-box;
  position: absolute;
  overflow: hidden;
  background-color: var(--nu-map--color--lighter-gray);
  padding: 0;
  height: var(--nu-map-location-info--height);
  width: var(--nu-map-location-info--width);
  color: var(--global--color-black);
}

.nu-map-location-info__toggle {
  background: var(--global--color-black);
  border: none;
  color: var(--global--color-white);
  cursor: pointer;
  height: var(--nu-map-location--header--height);
  left: 0;
  position: absolute;
  top: 0;
  width: 42px;
}

.nu-map-location-info__toggle-icon {
  display: block;
  transition: transform 300ms ease-out;
  transform: rotateZ(0deg);
}


.nu-map-location-title {
  cursor: pointer;
  font-family: var(--global--font-display-impact);
  font-size: var(--global--font-size-s);
  font-weight: var(--global--font-weight-ultra-black);
  line-height: var(--nu-map-location--header--height);
  margin-top: 0;
  height: var(--nu-map-location--header--height);
  background: var(--global--color-red);
  color: var(--global--color-white);
}

.nu-map-location-text {
  padding: 0 var(--global--spacing-small) var(--global--spacing-small);
}

#nu-map-effects {
  z-index: 2;
  position: absolute;
  top: 0;
  left: 0;
  -webkit-user-select: none; /* Safari */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* IE10+/Edge */
  user-select: none; /* Standard */
  -moz-user-focus: ignore;
  user-focus: none;
  pointer-events: none;
}


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
	/* width: 100vw; */
		width: 1024px;
    height: 600px;

	/* width: 1024px;
	height: 768px; */

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


/* background-image: url("https://img.icons8.com/ios/50/000000/building.png"); */
   background-repeat: no-repeat;
     background-position: center; /* Center the image */
background-size: 40px;
background-color: #E5D4AB;
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

@media only screen and (max-width: 1007px) {
  :root {
    --nu-map-button-icon--size: 50px;
    --nu-map-navigation--width: 180px;
  }
  .nu-map-location-info {
    bottom: calc(-1 * var(--nu-map-location-info--height) + var(--nu-map-location--header--height));
    left: calc(50% - 0.5 * var(--nu-map-location-info--width));
    border-radius: var(--global--border-radius-regular) var(--global--border-radius-regular) 0 0;
    transition: bottom 300ms ease-out;
  }

  .nu-map-location-info.is-active {
    bottom: 0;
}

    .nu-map-location-info__toggle-icon {
      transform: rotateZ(180deg);
    }


  .nu-map-location-title {
    padding-left: calc(42px + var(--global--spacing-unit));
  }

  .nu-map-navigation {
    border-radius: 0 0 0 var(--global--border-radius-regular);

  }
  .nu-map-button + .nu-map-button {
    margin-left: var(--global--spacing-small);
  }
}

</style>
<!-- <main> -->

<div class = "category-title">Commencement 2020 Map</div>


<div class = "map-wrapper">

<svg class = "map-graphic" id = "graphic"
  xmlns="http://www.w3.org/2000/svg">

 <image href={src_images}  class = "commencement-map"/>
 {#each locations as location}


 <!-- Common use case: embed HTML text into SVG -->
 <foreignObject x="{location.x}" y="{location.y}" width="80" height="80" class = "foreign-marker">

   <!-- <span class = "marker" id = "mark-location" style="background-image: url('//news.northeastern.edu/interactive/2021/10/commencement-2021/images/icon_1.png')" xmlns="http://www.w3.org/1999/xhtml"></span> -->
   <span class = "marker" id = "mark-location" style="background-image:url('//news.northeastern.edu/interactive/2021/10/commencement-2021/images/icon_{location.id}.png" xmlns="http://www.w3.org/1999/xhtml"></span>



 </foreignObject>

  {/each}


</svg>

<div class="nu-map-navigation-wrapper">
    <canvas id="nu-map-effects"></canvas>
    <menu class="nu-map-navigation">
        <button aria-label="Previous location" class="nu-map-button nu-map-button--prev"
                id="nu-map-button-prev">
            ➜
        </button>
        <button aria-label="Next location" class="nu-map-button nu-map-button--next"
                id="nu-map-button-next">
            ➜
        </button>
    </menu>
    </div>
    <div class="nu-map-location-info is-active">
        <button aria-label="Open description box."
                class="nu-map-location-info__toggle"><span
                class="nu-map-location-info__toggle-icon">▲</span></button>
        <h3 class="nu-map-location-title"></h3>
        <p class="nu-map-location-text"></p>
    </div>

</div>

{#each locations as location}

<div class = "location-name" on:click={() => navTo(location.id)}>{location.name}</div>
{/each}
<!-- <div class = "location-name" on:click={() => navTo(3)}>location 3</div> -->
<!-- <div class = "location-name" on:click={() => navTo(5)}>location 5</div> -->


<!-- </main> -->
