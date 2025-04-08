<!-- Map to color redlining areas, over the map of Greater Boston area. 
Importing merged geojson: final_polygons_with_affordability.geojson
Using map from mapbox -->
<script>
    import mapboxgl from "mapbox-gl";
    import "../../node_modules/mapbox-gl/dist/mapbox-gl.css";
    import { onMount } from "svelte";
    
    mapboxgl.accessToken = "pk.eyJ1IjoiaXNhYmVsbGFyb21lcm8iLCJhIjoiY205MDF0bmxzMGpjMzJyb29wMWR0aGg2MSJ9.C2IOPTKDoYYnPV6lfEW3tA";
    
    onMount(() => {
        let map = new mapboxgl.Map({
            container: 'map2',
            style: 'mapbox://styles/isabellaromero/cm94sxl6v007201qtbowkhefq',
            center: [-71.1097, 42.3736],
            zoom: 9.5
        });
    
        map.on('style.load', () => {
            map.setFog({});
        });
    
        map.on('load', () => {
            // Load and deduplicate the GeoJSON
            //fetch('static/final_polygons_with_affordability.geojson')
            fetch('.static/updated_final_polygons_with_affordability_4.geojson')
                .then(response => response.json())
                .then(data => {
                    const uniqueFeaturesMap = new Map();
    
                    data.features.forEach(feature => {
                        const areaId = feature.properties.area_id;
                        if (!uniqueFeaturesMap.has(areaId)) {
                            uniqueFeaturesMap.set(areaId, feature);
                        }
                    });
    
                    const deduplicatedGeoJSON = {
                        type: 'FeatureCollection',
                        features: Array.from(uniqueFeaturesMap.values())
                    };
    
                    // Add deduplicated source to the map
                    map.addSource('redlining', {
                        type: 'geojson',
                        data: deduplicatedGeoJSON
                    });
    
                    // filter 
                    const getBaseFilter = (grade = 'all') => {
                    const filter = [
                        'all',
                        ['==', ['get', 'residential'], true],
                        ['has', 'normalized_affordability'],
                        ['>=', ['get', 'normalized_affordability'], 0],
                        ['<=', ['get', 'normalized_affordability'], 1]
                    ];

                    if (grade !== 'all') {
                        filter.push(['==', ['get', 'grade'], grade]);
                    }

                    return filter;
                };
                document.getElementById('categoryFilter').addEventListener('change', (event) => {
                const selected = event.target.value;
                map.setFilter('redlining', getBaseFilter(selected));
            });


                    // Add layer with solid color styling
                    map.addLayer({
                        id: 'redlining',
                        type: 'fill',
                        source: 'redlining',
                        paint: {
                            'fill-color': [
                                'case',
                                ['<', ['get', 'normalized_affordability'], 0.3], '#FF0000',
                                ['<', ['get', 'normalized_affordability'], 0.4], '#FFFF00',
                                '#00FF00' 
                            ],
                            'fill-opacity': 0.5,
                            'fill-outline-color': 'white'
                        },
                        filter: [
                            'all',
                            ['==', ['get', 'residential'], true],
                            ['has', 'normalized_affordability'],
                            ['>=', ['get', 'normalized_affordability'], 0],
                            ['<=', ['get', 'normalized_affordability'], 1]
                        ]
                    });
                    /*
                    map.addLayer({
                        id: 'city-labels',
                        type: 'symbol',
                        source: 'redlining',
                        layout: {
                            'text-field': [
                                'format',
                                ['upcase', ['get', 'city']], { 'font-scale': 0.8 }
                            ],
                            'text-size': 12,
                            'text-font': ['Open Sans Bold', 'Arial Unicode MS Bold'],
                            'text-anchor': 'center'
                        },
                        paint: {
                            'text-color': '#333',
                            'text-halo-color': 'white',
                            'text-halo-width': 1
                        },
                        filter: [
                            'all',
                            ['==', ['get', 'residential'], true],
                            ['has', 'city']
                        ]
                    });
                    */
    
                    // Add tooltip on click
                    map.on('click', 'redlining', function (e) {
                        if (e.features.length > 0) {
                            const feature = e.features[0];
                            const coordinates = e.lngLat;
                            const props = feature.properties;
    
                            const city = props.city ? props.city.charAt(0).toUpperCase() + props.city.slice(1).toLowerCase() : 'N/A';
                            const category = props.category ? props.category : 'N/A';

                            const averagePrice = props.average_price ? `$${parseFloat(props.average_price).toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ',')}` : 'N/A';
    
                            const medianIncome = props.estimated_median_income ? `$${parseFloat(props.estimated_median_income).toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ',')}` : 'N/A';
                            const affordability = props.normalized_affordability !== undefined ? props.normalized_affordability.toFixed(2) : 'N/A';
                            
                            const areaID = props.area_id ? props.area_id : 'N/A';

                            
                            const tooltipHtml = `
                                <strong> 🏙️ City:</strong> ${city}<br>
                                <strong> 🏷️ Category:</strong> ${category}<br>
                                <strong> 🏠 Average Housing Price:</strong> ${averagePrice}<br>
                                <strong> 💵 Median Household Income:</strong> ${medianIncome}<br>
                                <strong> 📊 Normalized Affordability Ratio:</strong> ${affordability}<br>
                                

                            `;
    
                            const tooltip = document.getElementById('tooltip');
                            tooltip.innerHTML = tooltipHtml;
                            tooltip.style.display = 'block';
                            tooltip.style.left = `${e.point.x + 50}px`;
                            tooltip.style.top = `${e.point.y+80}px`;


                        }
                    });
    
                    // Hide tooltip on mouse leave
                    map.on('mouseleave', 'redlining', function () {
                        document.getElementById('tooltip').style.display = 'none';
                    });
                    
                });
        });
    });
    </script>
    