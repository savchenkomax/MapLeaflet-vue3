<script setup lang="ts">
import { LMarker, LPopup } from '@vue-leaflet/vue-leaflet'
import type { PropType } from 'vue'

import type { TMark } from '../types'

const props = defineProps({
  mark: {
    type: Object as PropType<TMark>,
    required: true,
    default: () => ({})
  }
})
const emit = defineEmits(['selectMarker'])
</script>

<template>
  <LMarker
    :lat-lng="[props.mark.latitude, props.mark.longitude]"
    @click="emit('selectMarker', $event)"
  >
    <LPopup>{{ mark.name }}</LPopup>
  </LMarker>
</template>

<style scoped></style>
