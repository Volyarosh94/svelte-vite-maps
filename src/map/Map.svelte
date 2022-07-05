<script>
    import {onMount} from 'svelte';
import MapRange from './MapRange.svelte';
import MapSearch from './MapSearch.svelte';
    
    const circleOptions = {
        strokeColor: "#4285f4",
      strokeOpacity: 0.8,
      strokeWeight: 2,
      fillColor: "#4285f4",
      fillOpacity: 0.35,
    }

	let pointPosition = {
        lat: 55.694804, 
        lng: 21.186457
    }

    let pointRange;

    let container;
    let map;
    let circle;
    let zoom = 8;

    let place = null;

    const applyBounds = () => {
        if (!place || !place.geometry) return;
        const bounds = circle?.getBounds() ?? new google.maps.LatLngBounds();
        if (place.geometry.viewport) {
            bounds.union(place.geometry.viewport);
        } else {
            bounds.extend(place.geometry.location);
        }
        map.fitBounds(bounds);
    }

    const handlePointRangeChange = (pointRange, pointPosition, map) => {
        console.log("Point Range: ", pointRange, map);
        // circle?.setMap(null);
        if (!circle || !circle.map) {
            console.log("Creating new circle object");
            circle = new google.maps.Circle({
                ...circleOptions,
                map: map,
                center: pointPosition,
                radius: pointRange * 1000
            });
        }
        circle.setCenter(pointPosition);
        circle.setRadius(pointRange * 1000);
        applyBounds();
        console.log("Place", place);
    }

    $: handlePointRangeChange(pointRange, pointPosition, map);

	const handleSelectedResult = (newPlace) => {
		place = newPlace;
        pointPosition = place.geometry.location;
        console.log(newPlace);
	}

    onMount(async () => {
        map = new google.maps.Map(container, {
            zoom, 
            center: pointPosition
        });
    })

</script>

<style>
    h1 {
        text-align: center  ;
    }
    .map-container {
        height: 100%;
        width: 70%;
    }
    .search-container {
        width: 30%;
        max-height: 100vh;
        display: flex;
        flex-direction: column;
    }
    .places-item{ 
        padding: 12px 18px;
    }
    .places-item:hover{
        cursor: pointer;
        background: #00000033;
        transition: all .15s ease-out;
    }
    :global(.places-item.selected) {
        background: #00000022;
    }
</style>


<div class="search-container">
    {#if map}   
        <h1>Map searcher</h1>
        <MapSearch map={map} onSelectedResult={handleSelectedResult}/>
            {#if place}
            <div class="places-item">
                <h4>{place.name}</h4>
                <p>{place.formatted_address}</p>
            </div>
            <MapRange bind:value={pointRange}/>
            {/if}
    {/if}

</div>
<div class="map-container" bind:this={container}></div>
