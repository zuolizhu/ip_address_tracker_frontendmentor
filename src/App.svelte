<script>
	import { onMount } from 'svelte';
	import Map from './components/Map.svelte';
	import IPTracker from './components/IPTracker.svelte';
	import '../node_modules/leaflet/dist/leaflet.css';

	let ipFetched = false;
	let clientIp;
	let clientGeo;
	let location;

	onMount(async () => {
		// const initRes = await fetch(`https://api.ipify.org?format=json`);
		// clientIp = await initRes.json();
		// const clientRes = await fetch(`https://geo.ipify.org/api/v1?apiKey=at_Nr1EYCfMNqXK1E50NoR5TnhQqFlRV&ipAddress=${clientIp.ip}`);
		// clientGeo = await clientRes.json();
		// ip: 74.72.2.211
		// lat: 40.7493
		// lng: -73.8213
		// location = clientGeo.location;
		ipFetched = true;
	});

</script>


<main>
	<IPTracker/>
	{#if ipFetched}
		<!-- <p>Your ip is {clientIp.ip}</p> -->
		<Map location={location}/>
	{:else}
		<p>Loading ...</p>
	{/if}
</main>