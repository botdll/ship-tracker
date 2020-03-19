<template>
  <div class="map">
    <l-map
          :zoom="zoom"
          :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <v-marker-cluster>
        <l-marker
                v-for="(ship, index) in ships"
                :key="index"
                :lat-lng="latLng(ship.LAT, ship.LON)">
          <l-icon :icon-size="ship.iconSize" :icon-url="icon"></l-icon>
          <l-tooltip>LAT: {{ ship.LAT }}	&#124; LON: {{ ship.LON}}</l-tooltip>
        </l-marker>
      </v-marker-cluster>
    </l-map>
  </div>
</template>

<script>
import L from 'leaflet';
import { LMap, LTileLayer, LMarker, LIcon, LTooltip } from 'vue2-leaflet';
import { Icon } from 'leaflet';
import Vue2LeafletMarkerCluster from 'vue2-leaflet-markercluster'
import shipImage from '../assets/ship.png'

delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
});

export default {
  name: 'ShipMap',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LIcon,
    LTooltip,
    'v-marker-cluster': Vue2LeafletMarkerCluster
  },
  props: {
    ships: Array
  },
  data() {
    return {
      zoom: 2,
      center: L.latLng(34.583069, -40.890467),
      icon: shipImage,
      iconSize: [25, 25],
      url: `https://tile.thunderforest.com/transport/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_MAP_API_KEY}`,
      attribution: 'Maps&copy; <a href="www.thunderforest.com">Thunderforest</a> | Data&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
    }
  },
  methods: {
    latLng(lat, lng) {
      return L.latLng(lat, lng);
    }
  }      
}
</script>

<style scoped>
@import "~leaflet.markercluster/dist/MarkerCluster.css";
@import "~leaflet.markercluster/dist/MarkerCluster.Default.css";

.map {
  height: 81vh;
}
</style>