<script>
	import Map from './components/Map.svelte';
	import IPTracker from './components/IPTracker.svelte';
	import '../node_modules/leaflet/dist/leaflet.css';

	let lat;
	let lng;

	function handleUpdateResult(event) {
		lat = event.detail.result.lat;
		lng = event.detail.result.lng;
	}
</script>

<main>
	<IPTracker on:updateResult={handleUpdateResult}/>
	<Map lat={lat} lng={lng}/>
</main>