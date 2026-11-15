<template>
  <Paranoma :scenes="scenes" initial-scene-id="kingdom" />
</template>

<script setup lang="ts">
import Paranoma from '~/components/paranoma/Paranoma.vue'

type SceneKey = 'kingdom' | 'inside_market' | 'inside_house_of_the_man' | 'inside_castle'

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
  kingdom: {
    name: 'THE KINGDOM OF ELDORIA',
    category: 'KINGDOM SQUARE',
    texture: '/img/paranoma/kingdom/kingdom.png',
    hotspots: [
      {
        id: 1,
        title: 'Enter the marketplace',
        type: 'teleport',
        targetScene: 'inside_market',
        coords: { x: 260, y: -80, z: -350 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 2,
        title: 'Visit the royal castle',
        type: 'teleport',
        targetScene: 'inside_castle',
        coords: { x: -280, y: 20, z: -340 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 3,
        title: 'The walled city',
        type: 'info',
        description: 'A medieval town gathers beneath the royal fortress, with bustling market stalls, stone walls, and distant mountain peaks.',
        coords: { x: 0, y: 160, z: -420 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
    ],
  },
  inside_market: {
    name: 'THE KINGDOM MARKET',
    category: 'MARKET STREET',
    texture: '/img/paranoma/kingdom/inside_market.png',
    hotspots: [
      {
        id: 4,
        title: 'Enter the house of the man',
        type: 'teleport',
        targetScene: 'inside_house_of_the_man',
        coords: { x: 350, y: 0, z: -280 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 5,
        title: 'Return to the kingdom square',
        type: 'teleport',
        targetScene: 'kingdom',
        coords: { x: -350, y: -30, z: -280 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 6,
        title: 'Visit the royal castle',
        type: 'teleport',
        targetScene: 'inside_castle',
        coords: { x: 0, y: 30, z: -430 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 7,
        title: 'The merchants’ street',
        type: 'info',
        description: 'Merchants, craftspeople, and travelers fill the market street beneath the towers of the royal castle.',
        coords: { x: 0, y: 120, z: -430 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
    ],
  },
  inside_house_of_the_man: {
    name: 'THE MERCHANT’S HOUSE',
    category: 'HOUSE INTERIOR',
    texture: '/img/paranoma/kingdom/inside_house_of_a_man.png',
    hotspots: [
      {
        id: 8,
        title: 'Return to the market',
        type: 'teleport',
        targetScene: 'inside_market',
        coords: { x: 0, y: -30, z: -420 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 9,
        title: 'A humble home',
        type: 'info',
        description: 'A warm timber-framed home with a hearth, simple furnishings, and everyday goods brought in from the nearby market.',
        coords: { x: 0, y: 100, z: -420 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
    ],
  },
  inside_castle: {
    name: 'THE ROYAL CASTLE',
    category: 'GREAT HALL',
    texture: '/img/paranoma/kingdom/inside_castle.png',
    hotspots: [
      {
        id: 10,
        title: 'Return to the kingdom square',
        type: 'teleport',
        targetScene: 'kingdom',
        coords: { x: -350, y: -30, z: -280 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 11,
        title: 'Walk to the marketplace',
        type: 'teleport',
        targetScene: 'inside_market',
        coords: { x: 350, y: -30, z: -280 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
      {
        id: 12,
        title: 'The great hall',
        type: 'info',
        description: 'The royal great hall is set for a feast, with banners, armor, long tables, and warm light from the hearths.',
        coords: { x: 0, y: 130, z: -420 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
    ],
  },
}
</script>
