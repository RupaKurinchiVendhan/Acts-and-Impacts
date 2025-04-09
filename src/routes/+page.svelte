<svelte:head>
  <title>FP2 - Acts and Impacts</title>
</svelte:head>

<script>
//import Redliningmap_display from '$lib/Redliningmap_display.svelte';
//import Redliningmap_map_v1 from '$lib/Redliningmap_map_v1.svelte';
//import Redliningmap_map_v2 from '$lib/Redliningmap_map_v2.svelte';
//import Redliningmap_map_v3 from '$lib/Redliningmap_map_v3.svelte';

//map n1
import Redliningmap_map_works_1 from '$lib/Redliningmap_map_works_1.svelte';

//map n2
import Redliningmap_map_works_2 from '$lib/Redliningmap_map_works_2.svelte';


{/* comment the map -- in file Redliningmap_map_works_v1.svelte
import mapboxgl from "mapbox-gl";
import "../../node_modules/mapbox-gl/dist/mapbox-gl.css";
mapboxgl.accessToken = "pk.eyJ1IjoiaXNhYmVsbGFyb21lcm8iLCJhIjoiY205MDF0bmxzMGpjMzJyb29wMWR0aGg2MSJ9.C2IOPTKDoYYnPV6lfEW3tA";

import { onMount } from "svelte";

onMount(() => {

    //load the map from mapbox
	let map = new mapboxgl.Map({
		container: 'map', // container ID
        //style: 'mapbox://styles/mapbox/streets-v11', // style URL
        style: 'mapbox://styles/isabellaromero/cm907b6nk007m01s06zlf5bhm',
        center: [-71.1097, 42.3736], // starting position [lng, lat]
        zoom: 10 // starting zoom
	});

    //try this code to add the json 
    map.on('style.load', () => {
        map.setFog({}); 
    });
    map.on('load', () => {
        map.addSource('redlining', {
            type: 'geojson',
            data: 'static/mapping_inequality_redlining.json'
        });

    // color the cities based on 'fill'    and filter only residential  
    map.addLayer({
    'id': 'redlining',
    'type': 'fill',
    'source': 'redlining',
    'paint': {
        'fill-color': [
            'get', 'fill' // color the cities based on the fill
        ],
        'fill-opacity': 0.5,  
        'fill-outline-color': 'white'  
    }, 
    'filter': ['==', ['get', 'residential'], true]  // filter only residential

    });
    });

    // Adding click event
    map.on('click', 'redlining', function (e) {
        if (e.features.length > 0) {
            const feature = e.features[0];
            const coordinates = e.lngLat;

            // Ensure that if the map is zoomed out such that multiple
            // copies of the feature are visible, the popup appears
            // over the copy being pointed to.
            while (Math.abs(e.lngLat.lng - coordinates.lng) > 180) {
                coordinates.lng += e.lngLat.lng > coordinates.lng ? 360 : -360;
            }

            // Update and position the tooltip
            document.getElementById('tooltip').innerHTML = feature.properties.city; // city name
            document.getElementById('tooltip').style.display = 'block';
            document.getElementById('tooltip').style.left = `${e.point.x}px`;
            document.getElementById('tooltip').style.top = `${e.point.y}px`;
        }
    });

    // Optional: Hide the tooltip when the mouse leaves the map
    map.on('mouseleave', 'redlining', function () {
        document.getElementById('tooltip').style.display = 'none';
    });
})
*/}
</script>

 <!--
<Redliningmap_map_works_1 /> -->

<Redliningmap_map_works_2 />


<style>
    @import url("$lib/global.css");
   
</style>

<!-- <h1>FP2 Interactive Map</h1>
<h2>Housing affordability ratio in historically redlining areas</h2> -->
 

<!--
<p> Redlining map 1. Fill cities based on status. Redlining json used </p>
<div id="map" style="height: 800px; margin-bottom: 20px;"></div>
-->
<body>
  

    <div id="tooltip"></div>
    <div id="scroll-container">
        <section class="outro first-section">
          <div>
            <p>Housing affordability is shaped by how housing prices compare to income.</p>
            <img src="ratio.gif" alt="Housing affordability ratio illustration"
                 style="margin-top: 16px; max-width: 50%; height: auto;" />
          </div>
          <p>Explore housing affordability by neighborhood in the Greater Boston area below.</p>
      
          <!-- Arrow Button (inside only this section) -->
          <button id="scroll-arrow" aria-label="Scroll to map">&#8595;</button>
        </section>
      
        <section class="map-section">
            <div class="container" style="position: relative;">
              <div id="toolbox-container">
                <label for="categoryFilter">TRY FILTERING BY REDLINING CATEGORY:</label>
                <select id="categoryFilter">
                  <option value="all">ALL CATEGORIES</option>
                  <option value="A">BEST</option>
                  <option value="B">STILL DESIRABLE</option>
                  <option value="C">DEFINITELY DECLINING</option>
                  <option value="D">HAZARDOUS</option>
                </select>
          
                <!-- Dropdown toggle -->
                <div id="dropdown-toggle" class="dropdown-toggle">
                  <span>Want more information on how housing affordability is calculated?</span>
                  <span class="arrow">&#9660;</span>
                </div>
          
                <!-- Hidden content -->
                <div id="dropdown-content" class="dropdown-content">
                    <p>The average annual housing price is calculated from the total housing price using the mortgage loan amortization calculation.</p>
                    <p>The monthly payment of a house is calculated using the formula: 
                        <strong>M = P × (r(1 + r)<sup>n</sup>) / ((1 + r)<sup>n-1</sup>)</strong>, where:
                        <ul>
                            <li><strong>P</strong> is the total price after an 18% downpayment,</li>
                            <li><strong>r</strong> is the monthly interest rate (3.5%),</li>
                            <li><strong>n</strong> is the number of payments (30 years).</li>
                        </ul>
                    <p>The annual housing price per neighborhood would be 12 times this monthly payment number. Estimated median household income is extracted from 2020 census data, based on records per city.</p>
                </div>
              </div>
          
              <div id="map2-container">
                <h2>Housing Affordability per Neighborhood in Greater Boston, in 2020</h2>
                <!-- <div id="map-title-overlay">Housing Affordability in Redlined Neighborhoods</div> -->
                <div id="map2"></div>
              </div>
              
              <div id="map-legend">
                <div><span class="legend-color" style="background-color: #00FF00;"></span> MORE AFFORDABLE</div>
                <div><span class="legend-color" style="background-color: #FFFF00;"></span> MODERATELY AFFORDABLE</div>
                <div><span class="legend-color" style="background-color: #FF0000;"></span> LESS AFFORDABLE</div>
              </div>
            </div>
          </section>
          
      
      
    <script>
        document.getElementById("scroll-arrow").addEventListener("click", () => {
        const scrollContainer = document.getElementById("scroll-container");
        const nextSection = scrollContainer.querySelectorAll("section")[1];
        nextSection.scrollIntoView({ behavior: "smooth", block: "start" });
        });
    
        // Dropdown toggle logic
        document.getElementById("dropdown-toggle").addEventListener("click", () => {
        const toggle = document.getElementById("dropdown-toggle");
        const content = document.getElementById("dropdown-content");
        toggle.classList.toggle("open");
        content.classList.toggle("open");
        });
    </script>
      
  
  </body>








