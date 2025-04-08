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


<div id="tooltip"></div>
<div class="scroll-wrapper">
    <section class="outro">
        <div>
          <p>
            Housing affordability is shaped by how housing prices compare to income.
          </p>
          
          <img src=".static/ratio.gif" alt="Housing affordability ratio illustration" style="margin-top: 16px; max-width: 50%; height: auto;" />
        </div>
        <p>
            Explore housing affordability by neighborhood in the Greater Boston area below.
          </p>
      </section>
      
  
    <section class="map-section">
        <div class="container" style="position: relative;">

            <!-- filter menu-->
            <div id="toolbox-container">
                <label for="categoryFilter">FILTER BY REDLINING CATEGORY:</label>
                <select id="categoryFilter">
                    <option value="all">ALL CATEGORIES</option>
                    <option value="A">BEST</option>
                    <option value="B">STILL DESIRABLE</option>
                    <option value="C">DEFINITELY DECLINING</option>
                    <option value="D">HAZARDOUS</option>
                </select>
            </div>
            
        
            <!-- map -->
            <div id="map2"></div>
            
        
        </div>
    </section>


    <!-- <section class="outro">
        <div>
          <p>
            Housing affordability is shaped by how housing prices compare to income.
          </p>
          
          <img src="static/ratio.gif" alt="Housing affordability ratio illustration" style="margin-top: 16px; max-width: 50%; height: auto;" />
        </div>
        <p>
            Explore housing affordability by neighborhood in the Greater Boston area below.
          </p>
      </section> -->
  </div>







