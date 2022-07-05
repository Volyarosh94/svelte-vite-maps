<script>
import { onMount } from "svelte";

    export let onSelectedResult;
    export let map;
    let searchInput;
    let searchBox;

    const findPlace = () => {
        const places = searchBox.getPlaces();
            if (places.length === 0) return;
            let bounds = new google.maps.LatLngBounds();
            
            const place = places[0];
            
            if (!place.geometry || !place.geometry.location) return;
            const marker = new google.maps.Marker({
                    map,
                    title: place.name,
                    position: place.geometry.location
                });
            
            if (place.geometry.viewport) {
                bounds.union(place.geometry.viewport);
            } else {
                bounds.extend(place.geometry.location)
            }
            
            map.fitBounds(bounds);
            onSelectedResult(place);
    }

    onMount(async () => {
        searchBox = new google.maps.places.SearchBox(searchInput);

        map.addListener('bounds_changed', () => {
            searchBox.setBounds(map.getBounds());
        })

        searchBox.addListener("places_changed", findPlace);
    });

</script>
<style>
    .search-container {
        display: flex;
        justify-content: start;
        align-items: center;
    }
    .search-input {
        border: 1px solid #33aaaa;
        border-radius: 12px;
        padding: 8px 12px;
        /* max-width: 300px; */
        width: 100%;
        margin: 6px 18px 18px;
    }
</style>
<div class="search-container">
    <input type="text" class="search-input" bind:this={searchInput}>
</div>