<template>
  <Paranoma :scenes="scenes" initial-scene-id="outdoor" />
</template>

<script setup lang="ts">
import Paranoma from '~/components/paranoma/Paranoma.vue'

type SceneKey = 'outdoor' | 'cyber_mall' | 'inside_bar'

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

// 3 VR360 Scenes Configuration
const scenes: Record<SceneKey, { name: string; category: string; texture: string; hotspots: Hotspot[] }> = {
  outdoor: {
    name: 'NEO-KYOTO CITY SKYLINE',
    category: 'OUTDOOR BALCONY',
    texture: '/image/paranoma.png',
    hotspots: [
      {
        id: 1,
        title: 'ENTER CYBER MALL ("Neo-Kyoto Mall")',
        type: 'teleport',
        targetScene: 'cyber_mall',
        coords: { x: -220, y: -40, z: -350 },
        screenX: 0,
        screenY: 0,
        visible: false
      },
      {
        id: 2,
        title: 'ENTER CYBER BAR ("The Chronos Club")',
        type: 'teleport',
        targetScene: 'inside_bar',
        coords: { x: 300, y: -120, z: -280 },
        screenX: 0,
        screenY: 0,
        visible: false
      },
      {
        id: 3,
        title: 'NEO-KYOTO Central Citadel',
        type: 'info',
        description: 'The monumental core of Neo-Kyoto featuring high-density vertical architecture, energy conduits, and mega-holographic displays.',
        coords: { x: 0, y: 100, z: -400 },
        screenX: 0,
        screenY: 0,
        visible: false
      },
      {
        id: 4,
        title: 'Elevated Monorail Network',
        type: 'info',
        description: 'Autonomous maglev sky-trains operating at 300 km/h, interconnecting upper urban strata and industrial hubs across the metropolis.',
        coords: { x: -350, y: -20, z: -250 },
        screenX: 0,
        screenY: 0,
        visible: false
      }
    ]
  },
  cyber_mall: {
    name: 'NEO-KYOTO STATION CYBER MALL',
    category: 'MALL & ARCADE DOME',
    texture: '/image/neokyoto-paranoma.png',
    hotspots: [
      {
        id: 20,
        title: 'ENTER THE CHRONOS BAR',
        type: 'teleport',
        targetScene: 'inside_bar',
        coords: { x: 320, y: -50, z: -250 },
        screenX: 0,
        screenY: 0,
        visible: false
      },
      {
        id: 21,
        title: 'EXIT TO CITY SKYLINE',
        type: 'teleport',
        targetScene: 'outdoor',
        coords: { x: -380, y: -20, z: -150 },
        screenX: 0,
        screenY: 0,
        visible: false
      },
      {
        id: 22,
        title: 'Cyber-Dojo & Virtual Arcade',
        type: 'info',
        description: 'Multi-level gaming complex featuring full-immersion neural VR martial arts and retro arcade simulators.',
        coords: { x: 0, y: 80, z: -400 },
        screenX: 0,
        screenY: 0,
        visible: false
      },
      {
        id: 23,
        title: 'Kurosawa Tech & Geisha-Bot 7 Store',
        type: 'info',
        description: 'Boutique outlet showcasing next-generation android assistant bots, cybernetic implants, and neural upgrades.',
        coords: { x: 250, y: 20, z: 320 },
        screenX: 0,
        screenY: 0,
        visible: false
      },
      {
        id: 24,
        title: 'Ghost-Shell Wear & Fashion Station',
        type: 'info',
        description: 'High-end cyberpunk apparel shop offering active-camo jackets, LED streetwear, and smart textiles.',
        coords: { x: -320, y: 40, z: 280 },
        screenX: 0,
        screenY: 0,
        visible: false
      }
    ]
  },
  inside_bar: {
    name: 'THE CHRONOS CLUB (BAR INTERIOR)',
    category: 'BAR INTERIOR',
    texture: '/image/inside_bar_paranoma.png',
    hotspots: [
      {
        id: 10,
        title: 'GO TO CYBER MALL & ARCADE',
        type: 'teleport',
        targetScene: 'cyber_mall',
        coords: { x: -320, y: -40, z: -240 },
        screenX: 0,
        screenY: 0,
        visible: false
      },
      {
        id: 11,
        title: 'EXIT TO CITY SKYLINE',
        type: 'teleport',
        targetScene: 'outdoor',
        coords: { x: 350, y: -20, z: -200 },
        screenX: 0,
        screenY: 0,
        visible: false
      },
      {
        id: 12,
        title: 'Android Bartender (Unit-7)',
        type: 'info',
        description: 'Serves rare synthetic spirits, glowing neon cocktails, and cybernetic energy brews to cyber-citizens and travelers.',
        coords: { x: 60, y: -40, z: -400 },
        screenX: 0,
        screenY: 0,
        visible: false
      },
      {
        id: 13,
        title: 'Live Synthwave Keytar Stage',
        type: 'info',
        description: 'Featuring nightly keytar solos and cyber-synth performances with responsive holographic light displays.',
        coords: { x: 420, y: 10, z: 150 },
        screenX: 0,
        screenY: 0,
        visible: false
      }
    ]
  }
}


</script>
