<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    let width = 800, height = 500;
    let svg;


    //let projection= d3.geoEquirectangular(); 
    //let geoGenerator= geoPatch().projection(projection); 
    //let u=select('#content g.map')
    //    .selectAll('path')
    //    .data(geojsonData.features)
    //    .join('path')
    //    .attr('d', geoGenerator);

    onMount(async () => {
        // Load GeoJSON Data
        const response = await fetch('/mapping_inequality_redlining.json');
        const geojsonData = await response.json();
        console.log("GeoJSON Loaded:", geojsonData);

        // Set up a Mercator projection (adjust the scale if needed)
        const projection = d3.geoMercator()
            .center([-71.0589, 42.3601]) // Longitude and latitude of Boston
            .scale(3000)
            .translate([width / 2, height / 2]);
        // Create the SVG container
        svg = d3.select("#map").append("svg")
            .attr("width", width)
            .attr("height", height);

        // Create a tooltip
        const tooltip = d3.select("#map").append("div")
            .attr("class", "tooltip")
            .style("position", "absolute")
            .style("background", "white")
            .style("padding", "5px")
            .style("border", "1px solid black")
            .style("border-radius", "3px")
            .style("visibility", "hidden");

        // Draw MultiPolygon and Polygon shapes
        svg.selectAll("path")
            .data(geojsonData.features)
            .enter().append("path")
            .attr("d", d => {
                if (!d.geometry || !d.geometry.coordinates) return null;
                return pathGenerator(d); // Convert lat/lon into a shape
            })
            .attr("fill", d => d.properties.fill || "#cccccc") 
            .attr("stroke", "#000")
            .attr("stroke-width", 0.5)
            .on("mouseover", (event, d) => {
                tooltip.style("visibility", "visible")
                    .text(`City: ${d.properties.city} | Category: ${d.properties.category}`)
                    .style("left", (event.pageX + 10) + "px")
                    .style("top", (event.pageY - 20) + "px");
            })
            .on("mouseout", () => tooltip.style("visibility", "hidden"));

        // Implement Zoom & Pan
        const zoom = d3.zoom()
            .scaleExtent([0.5, 5])
            .on("zoom", (event) => {
                svg.selectAll("path").attr("transform", event.transform);
            });

        svg.call(zoom);
    });
</script>

<div id="map"></div>

<style>
    #map {
        width: 800px;
        height: 500px;
        position: relative;
        border: 1px solid black;
    }

    .tooltip {
        position: absolute;
        background: white;
        padding: 5px;
        border: 1px solid black;
        border-radius: 3px;
        visibility: hidden;
    }
</style>
