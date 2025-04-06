
<!-- Map to color redlining areas, over the map of Greater Boston area. 
 Importing redlining json: mapping_inequality_redlining.json 
 Using map from mapbox-->

<script>
//code to use mapbox     
import mapboxgl from "mapbox-gl";
import "../../node_modules/mapbox-gl/dist/mapbox-gl.css";
mapboxgl.accessToken = "pk.eyJ1IjoiaXNhYmVsbGFyb21lcm8iLCJhIjoiY205MDF0bmxzMGpjMzJyb29wMWR0aGg2MSJ9.C2IOPTKDoYYnPV6lfEW3tA";

//start map 
import { onMount } from "svelte";

onMount(() => {

    //load the map from mapbox
	let map = new mapboxgl.Map({
		container: 'map2', // container ID
        style: 'mapbox://styles/mapbox/streets-v11', // style URL
        //style: 'mapbox://styles/isabellaromero/cm907b6nk007m01s06zlf5bhm', //custom map from mapbox
        center: [-71.1097, 42.3736], // starting position [lng, lat]
        zoom: 10 // starting zoom
	});

    //Code to add the json 
    map.on('style.load', () => {
        map.setFog({}); 
    });
    map.on('load', () => {
        map.addSource('redlining', {
            type: 'geojson',
            data: 'static/final_polygons_with_affordability.geojson'
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
            document.getElementById('tooltip').innerHTML = feature.properties.muni; // city name
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
</script>