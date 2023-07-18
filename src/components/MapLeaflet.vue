<script setup lang="ts">
import { ref } from 'vue'

import axios from 'axios'
import 'leaflet/dist/leaflet.css'
import { LMap, LTileLayer } from '@vue-leaflet/vue-leaflet'

import MarkerLeaflet from './MarkerLeaflet.vue'
import type { TMark } from '../types'

const zoom = ref(5)
const center = ref([48.7823, 9.17702])
const url = ref('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png')
const markers = ref<TMark[]>([])

axios
  .get(
    'https://raw.githubusercontent.com/waliot/test-tasks/master/assets/data/frontend-1-dataset.json'
  )
  .then((response) => {
    markers.value = response.data
  })

const selectMarker = (evt: any) => {
  center.value = [evt.latlng.lat, evt.latlng.lng]
  setTimeout(() => (zoom.value = 10), 500)
}
</script>

<template>
  <div class="wrapper">
    <LMap ref="map" v-model:zoom="zoom" v-model:center="center" :useGlobalLeaflet="false">
      <LTileLayer :url="url" />
      <MarkerLeaflet
        v-for="mark in markers"
        :key="mark.id"
        :mark="mark"
        @selectMarker="selectMarker($event)"
      />
    </LMap>
  </div>
</template>

<style scoped>
.wrapper {
  height: 700px;
  width: 1200px;
  margin: 0 auto;
}
</style>
