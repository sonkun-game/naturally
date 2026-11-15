<template>
  <Paranoma :scenes="scenes" initial-scene-id="ocean" />
</template>

<script setup lang="ts">
import Paranoma from '~/components/paranoma/Paranoma.vue'

type SceneKey = 'ocean' | 'shipwreck' | 'coral'

interface Hotspot {
  id: number
  title: string
  type: 'info' | 'teleport'
  targetScene?: SceneKey
  description?: string
  coords: { x: number; y: number; z: number }
  screenX: number
  screenY: number
  visible: boolean
}

const scenes: Record<SceneKey, { name: string; category: string; texture: string; hotspots: Hotspot[] }> = {
  ocean: {
    name: 'THE AZURE DEEP',
    category: 'OPEN OCEAN',
    texture: '/img/paranoma/ocean/ocean.png',
    hotspots: [
      {
        id: 1,
        title: 'Explore the shipwreck',
        type: 'teleport',
        targetScene: 'shipwreck',
        coords: { x: 300, y: -50, z: -300 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 2,
        title: 'Visit the coral garden',
        type: 'teleport',
        targetScene: 'coral',
        coords: { x: -320, y: -80, z: -240 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 3,
        title: 'Sunlit waters',
        type: 'info',
        description: 'Sunbeams filter through the water above a vibrant reef, where schools of fish move between the coral and the old wreck.',
        coords: { x: 0, y: 220, z: -420 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
    ],
  },
  shipwreck: {
    name: 'THE LOST SHIP',
    category: 'SHIPWRECK INTERIOR',
    texture: '/img/paranoma/ocean/inside_wreckship.png',
    hotspots: [
      {
        id: 4,
        title: 'Return to the open ocean',
        type: 'teleport',
        targetScene: 'ocean',
        coords: { x: 0, y: -30, z: -420 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 5,
        title: 'Swim to the coral garden',
        type: 'teleport',
        targetScene: 'coral',
        coords: { x: 380, y: -60, z: -220 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 6,
        title: 'A vessel reclaimed by the sea',
        type: 'info',
        description: 'The ship’s broken ribs and scattered timbers have become shelter for marine life in the quiet depths.',
        coords: { x: 0, y: 80, z: -420 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
    ],
  },
  coral: {
    name: 'THE CORAL GARDEN',
    category: 'REEF CAVERN',
    texture: '/img/paranoma/ocean/inside_coral.png',
    hotspots: [
      {
        id: 7,
        title: 'Return to the open ocean',
        type: 'teleport',
        targetScene: 'ocean',
        coords: { x: 0, y: -30, z: -420 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 8,
        title: 'Explore the shipwreck',
        type: 'teleport',
        targetScene: 'shipwreck',
        coords: { x: -360, y: -60, z: -250 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 9,
        title: 'Coral reef ecosystem',
        type: 'info',
        description: 'A sheltered reef cavern filled with branching corals, colorful fish, and shafts of light from the water above.',
        coords: { x: 0, y: 100, z: -420 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
    ],
  },
}
</script>
