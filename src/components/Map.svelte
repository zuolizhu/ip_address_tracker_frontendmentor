<script>
  import * as Leaflet from 'leaflet';
  import { afterUpdate } from 'svelte';

  let map;
  let mapCtrl;
  let marker;
  
  // default location is set to New York City
  export let lat = 40.7167;
  export let lng = -74.0067;

  const Location_Marker = Leaflet.icon({
    iconUrl: '/assets/images/icon-location.svg',
    iconSize: [46, 56]
  });

  function mapAction(container) {
    map = createMap(container);
    return {
      destroy: () => {
        map.remove();
      }
    }
  }

  afterUpdate(() => {
    map.panTo([lat, lng], 14);
    marker.setLatLng([lat, lng]);
  });

  function createMap(container) {
    // map init center
    mapCtrl = Leaflet.map(container, { zoomControl: false }).setView([lat, lng], 14);
    // map tile layer
    Leaflet.tileLayer('https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png', {
      attribution: `&copy;<a href="https://www.openstreetmap.org/copyright" target="_blank">OpenStreetMap</a>`,
      subdomains: 'abcd',
      maxZoom: 14,
    }).addTo(mapCtrl);

    // marker on the map
    marker = Leaflet.marker(mapCtrl.getCenter(), {
      icon: Location_Marker
    }).addTo(mapCtrl);

    return mapCtrl;
  }

  function resizeMap() {
    if (map) { map.invalidateSize(); }
  }
</script>

<svelte:window on:resize={resizeMap} />

<section>
  <h2 class="sr-only">map section</h2>
  <div class="map-container" use:mapAction></div>
</section>

<style>
  .map-container {
    width:100%;
    height: calc(100vh - 30rem);
  }
  @media (min-width: 768px) {
    .map-container {
      height: calc(100vh - 28rem);
    }
  }
</style>