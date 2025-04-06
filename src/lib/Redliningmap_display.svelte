<script>
    import { onMount } from 'svelte';
    let geojsonData = {};
  
    onMount(async () => {
      try {
        // Adjust the path to where your GeoJSON data is stored
        const response = await fetch('/static/mapping_inequality_redlining.json');
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        geojsonData = await response.json();
        console.log("Data loaded", geojsonData);
      } catch (error) {
        console.error("Failed to load or parse the JSON", error);
      }
    });
  </script>
  
  <!-- Display a simple list of GeoJSON features -->
  {#if geojsonData.features}
    <ul>
      {#each geojsonData.features as feature}
        <li>{feature.properties.city}</li> 
        <li>{feature.geometry.type}</li> 
      {/each}
    </ul>
  {:else}
    <p>No data loaded.</p>
  {/if}
  
  <style>
    ul {
      list-style-type: none;
      padding: 0;
    }
    li {
      margin: 5px 0;
      padding: 10px;
      background-color: #f0f0f0;
      border: 1px solid #ddd;
    }
  </style>
  