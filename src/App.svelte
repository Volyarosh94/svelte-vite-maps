<script>
import Map from "./map/Map.svelte";
	export let ready;

</script>

<svelte:head>
	<script defer async 
	src="https://maps.googleapis.com/maps/api/js?key=AIzaSyB5HYrOwNUgeMxMWUx3QGp8fev-PWjFoYw&libraries=places&callback=mapSetuped"
	></script>
</svelte:head>


<style>
	:global(body, *) {
		padding: 0;
		margin: 0;
		box-sizing: border-box;
	}
	:global(main) {
		height: 100vh;
		display: flex;
		flex-direction: column;
	}
	h1 {
		text-align: center;
		font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
		text-transform: uppercase;
		font-size: 3em;
	}
	.map-wrapper {
		width: 100%;
		flex: 1;
		display: flex;
		/* flex-direction: column; */
	}
</style>

<main>
	
	{#if ready}
	
	<div class="map-wrapper">
		<Map>
		</Map>
	</div>
	{/if}
	 
</main>
