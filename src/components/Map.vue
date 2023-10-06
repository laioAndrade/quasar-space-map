<template>
  <div class="mapContainer">
    <div id="map"></div>
  </div>
</template>

<script>
import { onBeforeUnmount, onMounted } from 'vue'
import 'leaflet/dist/leaflet.css'
import leaflet from 'leaflet'

import axios from 'axios'

export default {
  name: 'LMap',
  setup() {
    let lMap = null
    const wmsUrl = 'https://geoserver.meioambiente.mg.gov.br/master/IDE/ows?'
    const wmsMicroRegionLayer = 'IDE:ide_1102_mg_microrregioes_ibge_pol'

    onMounted(() => {
      createMapLayer()
    })

    onBeforeUnmount(() => {
      if (lMap != null) {
        lMap.remove()
        lMap = null
      }
    })

    const createMapLayer = () => {
      lMap = leaflet.map('map').setView([-18.5543759, -48.0922649], 7)

      leaflet.tileLayer('https://{s}.tile.osm.org/{z}/{x}/{y}.png', {}).addTo(lMap)

      leaflet.tileLayer
        .wms(wmsUrl, {
          layers: wmsMicroRegionLayer,
          opacity: 0.6,
          transparent: 'true'
        })
        .addTo(lMap)

      const overLayers = {
        'Domínios hidrogeológicos': leaflet.tileLayer.wms(wmsUrl, {
          layers: 'IDE:ide_1704_mg_dominios_hidrogeologicos_pol',
          opacity: 0.4,
          transparent: 'true'
        }),
        'Erodibilidade do solo': leaflet.tileLayer.wms(wmsUrl, {
          layers: 'IDE:ide_2401_mg_erodibilidade_pol',
          opacity: 0.4,
          transparent: 'true'
        }),
        'Temperatura média anual': leaflet.tileLayer.wms(wmsUrl, {
          layers: 'IDE:ide_2401_mg_temperatura_media_anual_pol',
          opacity: 0.4,
          transparent: 'true'
        })
      }

      const baseLayers = {}

      leaflet.control.layers(baseLayers, overLayers).addTo(lMap)
      lMap.on('click', getFeatureInfo)
    }

    const getFeatureInfo = async (e) => {
      const mapBounds = lMap.getBounds()
      const mapSize = lMap.getSize()
      const width = mapBounds.getNorthEast().lng - mapBounds.getSouthWest().lng
      const height = mapBounds.getNorthEast().lat - mapBounds.getSouthWest().lat
      const pointX = (((e.latlng.lng - mapBounds.getSouthWest().lng) / width) * mapSize.x).toFixed(
        0
      )
      const pointY = (((mapBounds.getNorthEast().lat - e.latlng.lat) / height) * mapSize.y).toFixed(
        0
      )

      const params = {
        service: 'WMS',
        version: '1.1.1',
        request: 'GetFeatureInfo',
        layers: wmsMicroRegionLayer,
        transparent: true,
        query_layers: wmsMicroRegionLayer,
        format: 'image/png',
        info_format: 'text/html',
        feature_count: 50,
        height: lMap.getSize().y,
        width: lMap.getSize().x,
        x: pointX,
        y: pointY,
        srs: 'EPSG:4674',
        bbox: lMap.getBounds().toBBoxString()
      }

      try {
        const response = await axios.get(wmsUrl, { params })

        leaflet.popup({ maxWidth: 800 }).setLatLng(e.latlng).setContent(response.data).openOn(lMap)
      } catch (error) {
        alert('Falha ao buscar dados')
      }
    }
  }
}
</script>

<style scoped>
.mapContainer {
  padding: 0px 0px;
  height: calc(100vh - 150px);
}
#map {
  width: 100%;
  height: 100%;
}

@media (max-width: 420px) {
  .mapContainer {
    padding: 0;
  }
}
</style>
