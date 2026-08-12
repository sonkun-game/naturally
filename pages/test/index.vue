<template>
    <div class="relative w-screen h-screen overflow-hidden bg-black font-sans select-none">
        <div ref="canvasContainer" class="w-full h-full cursor-grab active:cursor-grabbing"></div>
    </div>
</template>


<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import * as THREE from 'three'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'
import {
    Compass,
    Volume2,
    VolumeX,
    RotateCcw,
    Info,
    Sparkles,
    X,
    Play,
    Pause,
    ZoomIn,
    ZoomOut,
    Eye,
    Maximize,
    LogIn,
    LogOut,
    Building2,
    MapPin,
    ChevronDown
} from 'lucide-vue-next'

// xac dinh loai scene
type SceneKey = 'outdoor' | 'cyber_mall' | 'inside_bar'

const canvasContainer = ref<HTMLDivElement | null>(null)
const isLoading = ref(true)
const isTransitioning = ref(false)
const isLocationMenuOpen = ref(false)
const loadProgress = ref(0)
const isAutoRotate = ref(true)
const isAudioPlaying = ref(false)
const showHotspots = ref(true)
const currentFov = ref(75)
const currentHeading = ref(0)

const currentSceneId = ref<SceneKey>('outdoor')
const targetSceneId = ref<SceneKey | null>(null)

// tao diem hotspot
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

const activeHotspot = ref<Hotspot | null>(null)

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

const currentHotspots = ref<Hotspot[]>(scenes.outdoor.hotspots)


// Three.js Variables
let scene: THREE.Scene
let camera: THREE.PerspectiveCamera
let renderer: THREE.WebGLRenderer
let sphereMesh: THREE.Mesh
let sphereMaterial: THREE.MeshBasicMaterial
let animationFrameId: number
const loadedTextures: Record<string, THREE.Texture> = {}

// Rotation & Interaction Variables
let isUserInteracting = false
let onPointerDownPointerX = 0
let onPointerDownPointerY = 0
let onPointerDownLon = 0
let onPointerDownLat = 0
let lon = 0
let lat = 0
let phi = 0
let theta = 0

const initVR360 = () => {
    if (!canvasContainer.value) return

    const width = window.innerWidth
    const height = window.innerHeight

    // 1. Scene
    scene = new THREE.Scene()

    const gridHelper = new THREE.GridHelper()
    scene.add(gridHelper)

    // camera = new THREE.PerspectiveCamera(75, width / height, 1, 1100)

    camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
    camera.position.set(10, 10, 10)
    camera.lookAt(0, 0, 0)

    const geometry = new THREE.SphereGeometry(500, 60, 40)
    geometry.scale(-1, 1, 1) // Invert geometry so texture faces inward


    // Light
    const ambientLight = new THREE.AmbientLight(0xffffff, Math.PI);
    ambientLight.intensity = 3
    ambientLight.visible = true;
    scene.add(ambientLight);

    sphereMaterial = new THREE.MeshBasicMaterial()
    sphereMesh = new THREE.Mesh(geometry, sphereMaterial)
    scene.add(sphereMesh)

    renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false })
    renderer.setPixelRatio(window.devicePixelRatio)
    renderer.setSize(width, height)
    canvasContainer.value.appendChild(renderer.domElement)

    loadSceneTexture('outdoor', () => {
        isLoading.value = false
    })

    animate()
}

const loadSceneTexture = (sceneKey: SceneKey, callback?: () => void) => {
    const textureUrl = scenes[sceneKey].texture

    if (loadedTextures[textureUrl]) {
        sphereMaterial.map = loadedTextures[textureUrl]
        sphereMaterial.needsUpdate = true
        if (callback) callback()
        return
    }

    const textureLoader = new THREE.TextureLoader()
    textureLoader.load(
        textureUrl,
        (texture) => {
            texture.colorSpace = THREE.SRGBColorSpace
            loadedTextures[textureUrl] = texture
            sphereMaterial.map = texture
            sphereMaterial.needsUpdate = true
            if (callback) callback()
        },
        (xhr) => {
            if (xhr.lengthComputable) {
                loadProgress.value = Math.round((xhr.loaded / xhr.total) * 100)
            } else {
                loadProgress.value = 85
            }
        },
        (err) => {
            console.error('Error loading panorama image:', err)
            if (callback) callback()
        }
    )
}

const animate = () => {
    animationFrameId = requestAnimationFrame(animate)

    // if (isAutoRotate.value && !isUserInteracting && !isTransitioning.value) {
    //     lon += 0.08
    // }

    // lat = Math.max(-85, Math.min(85, lat))
    // phi = THREE.MathUtils.degToRad(90 - lat)
    // theta = THREE.MathUtils.degToRad(lon)

    // const x = 500 * Math.sin(phi) * Math.cos(theta)
    // const y = 500 * Math.cos(phi)
    // const z = 500 * Math.sin(phi) * Math.sin(theta)

    // camera.lookAt(x, y, z)

    // Update Compass Heading
    let heading = Math.round((lon % 360 + 360) % 360)
    // currentHeading.value = heading

    //   updateHotspots()

    renderer.render(scene, camera)
}

onMounted(() => {
    initVR360()
})


</script>