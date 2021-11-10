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
	  x: 680,
	  y: 1400,
},
  {  id: 2,
	  name: 'Easy Being Green',
		description: 'The Sculpture Garden holds a peaceful spot for a very green photo backdrop for photos of you and your loved ones.',
		x: 1628,
             y: 1333,
  },
  { id: 3,
	  name: 'Blowing in the Wind',
	  description: 'Krentzman Quad holds a great collection of pinwheels for fun photos!',
	  x: 1580,
             y: 784
  },
  {  id: 4,
	  name: 'You’re So Fancy!',
    description: 'Bright red sequins as a flashy background for you and friends at base of walking bridge between Snell and Egan!',
	x: 1290,
		   y: 1323
  },
  { id: 5,
	  name: 'N2020',
    description: 'Doesn’t matter what direction you’re headed in, stop by Snell Quad for a great N2020 photo op!',
	x: 1454,
	y: 1211
   }
];



onMount(() => {

	let mapWrapper = document.querySelector('.map-wrapper');

	let activeMarker = document.querySelector('.marker')
	console.log(activeMarker)

	let allMarkers = document.querySelectorAll('.marker')
	console.log(allMarkers)

	let locationInfo = document.querySelector('.nu-map-location-info');
	console.log(locationInfo)
	//
	// let info =  document.querySelectorAll('.nu-map-location-info');
	// console.log(info)
	// console.log( document.querySelectorAll('.nu-map-location-info'))
	let locationInfoTitle = document.querySelector('.nu-map-location-title');
	let locationInfoText = document.querySelector('.nu-map-location-text');
	let locationInfoToggle = document.querySelector('.nu-map-location-info__toggle');
	let prevButton = document.querySelector('.nu-map-button--prev');
	let nextButton = document.querySelector('.nu-map-button--next');
	// const mapWrapper = document.querySelector('.map-wrapper');
	let canvasElement = document.querySelector('#nu-map-effects');
	const canvasPath = canvasElement.getContext('2d');
	let minimumParticles = 5;
	let colors = ['#badb00', '#00cfb5', '#ff854f', '#ffbf3d', '#006eb5'];
	let particleInterval;
	let _particles = [];
	let activeLocation = 0;
    let tl = gsap.timeline({ease: Power2.in});

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


	// activeMarker.classList.remove('is-active');

        activateLocation(activeLocation);

        nextButton.addEventListener('click', () => {
            activateLocation(activeLocation + 1);
			// 	if (activeMarker.classList.contains('is-active')) {
			// 		activeMarker.classList.remove('is-active');
			// 	} else {
			// 	// allMarkers.forEach.call(function(el) {
			// 	// el.classList.remove("is-active");
			// 	// });
			// 	activeMarker.classList.add('is-active');
			// }
			if (activeMarker.classList.contains('is-active')) {
				activeMarker.classList.remove('is-active');
			} else {
				activeMarker.classList.add('is-active');
			}


        });

        prevButton.addEventListener('click', () => {
            activateLocation(activeLocation - 1);
			// activeMarker.classList.add('is-active');
			if (activeMarker.classList.contains('is-active')) {
				activeMarker.classList.remove('is-active');
			} else {
				activeMarker.classList.add('is-active');
			}

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
		// 	if (activeMarker.classList.contains('is-active')) {
		// 		// activeMarker.classList.remove('is-active');
		// 	} else {
		// 		allMarkers.forEach.call(function(el) {
		// 	el.classList.remove("is-active");
		// 	});
		// 	activeMarker.classList.add('is-active');
		// }

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

		navTo(index+1);

		activeLocation = index;
		let location = locations[index];
		locationInfoTitle.innerHTML = location.name;
		locationInfoText.innerHTML = location.description;
	}

	class Particles {
		   constructor(x, y, pSize, color) {
			   this.x = x
			   this.y = y
			   this.size = pSize
			   this.color = color
			   this.mass = 5 * Math.random() - 2.5;
			   this.dir = -1 * Math.random();

		   }

		   draw() {
			   canvasPath.beginPath()
			   if (0 > this.size) {
				   this.size = -1 * this.size;
			   }
			   canvasPath.arc(this.x, this.y, this.size, 0, Math.PI * 2)
			   canvasPath.fillStyle = this.color
			   canvasPath.fill()

		   }

		   update() {
			   this.y -= this.mass
			   this.x += this.dir
			   if (this.size > 0.1) {
				   this.size -= 0.2
			   }

		   }
	   }

	   function click() {
	        let boundingClientRect = this.getBoundingClientRect();

	        particleInterval = setInterval(() => {
	            for (let i = 0; i < minimumParticles; i++) {
	                // let x = boundingClientRect.x - (boundingClientRect.width / 2) + Math.random() * (40 - 40) + 40;
	                let x = boundingClientRect.x + (boundingClientRect.width / 2);
	                let y = boundingClientRect.y - (boundingClientRect.height / 2) + 50;
	                let color = colors[Math.floor(Math.random() * colors.length)];
	                let size = Math.random() * 20 + 10
	                _particles.push(new Particles(x, y, size, color))
	            }
	        }, 60);

	        tl.to(this,
	            {
	                scale: 0.8,
	                duration: 0.05
	            })
	            .to(this,
	                {
	                    scale: 1,
	                    duration: 0.05,
	                    onComplete: () => {
	                        clearInterval(particleInterval)
	                    }
	                })

	    }

		function spawnParticles() {
	         _particles.forEach((particle, index) => {

	             particle.draw()
	             particle.update()

	             if (particle.pSize < 1) {
	                 _particles.splice(index, 1)


	             }
	         })
	     }


	     function animateParticles() {
	         canvasPath.clearRect(0, 0, canvasElement.width, canvasElement.height)

	         nextButton.addEventListener('click', click)
	         prevButton.addEventListener('click', click)


	         spawnParticles()
	         requestAnimationFrame(animateParticles)
	     }

	     animateParticles()



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

:root{
	/**
	   * Global font jobs.
	   * Fonts should be set them to the job the text is doing. Avoid assigning
	   * specific font faces on elements to keep future updates simple.
	   *
	   * Body. Versatile and facilitates reading even long-form content comfortably.
	   *
	   * Display. To attract attention and set the tone. Good for top-level headings
	   * and hero text.
	   *
	   * Lettering. Artistic font for displaying an individual letter with flair.
	   *
	   * Signage. To help people find their way around. Use for labels in a grid of
	   * products, buttons, strong calls to action, and footers full of links.
	   *
	   * Small. For clarity at small sizes. Captions, labels, footnotes, or where
	   * there is limited screen real estate.
	   *
	   * Tabular. Quickly scannable to gather data and make comparisons. Ideal for
	   * tables of contents, charts, or anywhere data needs to be easily understood.
	   *
	   * Technical. For technical accuracy where glyph placement is crucial. Use
	   * when displaying math formulas, code, or even poetry and infographics.
	   *
	   * @since 1.5
	   */
	   --global--font-body: harriet, serif;
	   --global--font-display: akkurat, sans-serif;
	   --global--font-display-impact: trim-poster, sans-serif;
	   --global--font-signage: brauerneue, sans-serif;
	   --global--font-small: akkurat, sans-serif;
	   --global--font-tabular: akkurat, sans-serif;
	   --global--font-technical: akkurat, sans-serif;
	   --global--font-lettering: 'Pinyon Script', cursive;

	   /**
 * Global font sizes.
 * Specified in t-shirt sizing.
 *
 * @since 1.5
 */
--global--font-size-xxs: 12px; /* 12px */
--global--font-size-xs: 0.875rem; /* ~14px */
--global--font-size-s: 1rem; /* ~16px */
--global--font-size-m: 1.188rem; /* ~19px */
--global--font-size-l: 1.5rem; /* ~24px */
--global--font-size-xl: 1.875rem; /* ~30px */
--global--font-size-xxl: 2.25rem; /* ~36px */
--global--font-size-3xl: 2.5rem; /* 40px */
--global--font-size-4xl: 3rem; /* 48px */
--global--font-size-5xl: 4.063rem; /* ~65px */
--global--font-size-6xl: 4.5rem; /* ~72px */
/* Line Height */
--global--line-height-body: 1.4;
/* Content bounds */
--global--ceiling-text: 700px;
--global--ceiling-image: 750px;
--global--ceiling-content: 1150px;

/**
	  * Global font weights.
	  *
	  * @since 1.5
	  */
	 --global--font-weight-thin: 100;
	 --global--font-weight-extra-light: 200;
	 --global--font-weight-light: 300;
	 --global--font-weight-regular: 400;
	 --global--font-weight-medium: 500;
	 --global--font-weight-semi-bold: 600;
	 --global--font-weight-bold: 700;
	 --global--font-weight-extra-bold: 800;
	 --global--font-weight-black: 900;
	 --global--font-weight-ultra-black: 950;
	 /**
	  * Colors.
	  * The names for hues are those found on a basic RYB color wheel with tertiary
	  * colors.
	  *
	  * @see https://en.wikipedia.org/wiki/Tertiary_color#Traditional_painting_(RYB)
	  *
	  * Red, Red-Orange, Orange, Yellow-Orange, Yellow, Yellow-Green, Green,
	  * Blue-Green, Blue, Blue-Violet, Violet, and Red-Violet, plus Gray, White,
	  * and Black.
	  *
	  * Accessibility:
	  * White text will pass on colors with a "dark-" prefix.
	  * Black text will pass on colors with a "light-" prefix.
	  *
	  * Neutral colors:
	  * Neutral colors that are not true neutral have either a "warm" or "cool"
	  * prefix, as appropriate.
	  */
	 --global--color-darkest-red: #1f0406;
	 --global--color-dark-red: #7a0f19;
	 --global--color-red: #D41B2C;
	 --global--color-darker-blue: #1b3645;
	 --global--color-dark-blue: #385775;
	 --global--color-blue: #006EB5;
	 --global--color-light-blue: #9ebcda;
	 --global--color-lighter-blue: #f7fcfd;
	 --global--color-accent-blue: #52CFE5;
	 --global--color-blue-green: #00CFB5;
	 --global--color-violet: #6e016b;
	 --global--color-darkest-blue: #1b3645;
	 --global--color-teal: #00cfb5;
	 --global--color-purple: #6e016b;
	 --global--color-black: #000;
	 --global--color-white: #fff;
	 --global--color-darker-gray: #222;
	 --global--color-dark-gray: #6F7680;
	 --global--color-gray: #555;
	 --global--color-light-gray: #99a3b0;
	 --global--color-lighter-gray: #cbcccb;
	 --global--color-lightest-gray: #efefef;
	 --global--color-primary: var(--global--color-black);
	 --global--color-background: var(--global--color-white);
	 --global--color-secondary: var(--global--color-red);
	 --global--color-disabled: var(--global--color-lighter-gray);
	 --global--color-disabled-background: var(--global--color-lightest-gray);

	 /**
		* Global Spacing.
		*/
	   --global--spacing-unit: 16px;
	   --global--spacing-vertical: calc(5 * var(--global--spacing-unit));
	   --global--spacing-horizontal: calc(2.5 * var(--global--spacing-unit));
	   --global--spacing-gap: 20px;
	   --global--spacing-extra-small: calc(0.6 * var(--global--spacing-unit));
	   --global--spacing-small: calc(0.8 * var(--global--spacing-unit));
	   --global--spacing-medium: var(--global--spacing-unit);
	   --global--spacing-large: calc(1.5 * var(--global--spacing-unit));
	   --global--spacing-extra-large: calc(1.75 * var(--global--spacing-unit));
	   /**
	   * Global Borders
	   */
	   --global--border-color-light: var(--global--color-lightest-gray);
	   --global--border-color-regular: var(--global--color-lighter-gray);
	   --global--border-color-heavy: var(--global--color-gray);
	   --global--border-weight-regular: 1px;
	   --global--border-weight-bold: 2px;
	   --global--border-radius-tight: 3px;
	   --global--border-radius-regular: 8px;
	   --global--border-radius-loose: 16px;
}

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
.map-graphic {
        position: absolute;
        top: 0;
        left: 0;
        width: 3060px;
        height: 2031px;
        transition: top 2s ease 0s, left 2s ease 0s
    }

    image.commencement-map {
        width: 3060px;
        height: 2031px;
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

  .map-wrapper{
	  width: 100%;
  }
}

/* TODO: Missing media query for large viewport */
@media only screen and (min-width: 1008px) {
	:root {
             --nu-map-button-icon--size: 36px;
             --nu-map-navigation--width: var(--nu-map-location-info--width);
         }
         .nu-map-location-info {
             border-radius: 0 0 0 var(--global--border-radius-regular);
             right: 0;
             top: calc(var(--global--spacing-medium) + var(--nu-map-button-icon--size));
         }
         .nu-map-location-info__toggle {
             display: none;
         }
         .nu-map-location-title {
             padding: 0 var(--global--spacing-medium);
             text-align: center;
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
<!--
{#each locations as location}

<div class = "location-name" on:click={() => navTo(location.id)}>{location.name}</div>
{/each} -->
<!-- <div class = "location-name" on:click={() => navTo(3)}>location 3</div> -->
<!-- <div class = "location-name" on:click={() => navTo(5)}>location 5</div> -->


<!-- </main> -->
