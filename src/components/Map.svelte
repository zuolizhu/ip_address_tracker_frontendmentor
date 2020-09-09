<script>
  import * as Leaflet from 'leaflet';

  let map;
  let mapCtrl;
  let marker;
  export let location;

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

  function createMap(container) {
    // map init center
    mapCtrl = Leaflet.map(container).setView([location.lat, location.lng], 14);
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
  <div style="height:600px;width:100%" use:mapAction></div>
</section>