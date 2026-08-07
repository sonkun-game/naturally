<template>
  <div class="relative w-screen h-screen overflow-hidden bg-black font-sans select-none">
    <!-- VR Canvas Container -->
    <div ref="canvasContainer" class="w-full h-full cursor-grab active:cursor-grabbing"></div>

    <!-- Loading Screen -->
    <Transition
      enter-active-class="transition duration-500 ease-out"
      enter-from-class="opacity-0"
      enter-to-class="opacity-100"
      leave-active-class="transition duration-700 ease-in"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <div
        v-if="isLoading"
        class="fixed inset-0 z-50 flex flex-col items-center justify-center bg-gray-950 text-white"
      >
        <div class="relative flex items-center justify-center mb-8">
          <div class="w-24 h-24 border-4 border-cyan-500/20 border-t-cyan-400 rounded-full animate-spin"></div>
          <div class="absolute w-16 h-16 border-4 border-fuchsia-500/20 border-b-fuchsia-500 rounded-full animate-spin" style="animation-direction: reverse; animation-duration: 1.5s;"></div>
          <Compass class="w-8 h-8 text-cyan-400 absolute animate-pulse" />
        </div>
        <h2 class="font-serif text-3xl font-bold tracking-widest text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 via-fuchsia-400 to-emerald-400 mb-3 uppercase">
          NEO-KYOTO VR-360
        </h2>
        <p class="text-xs tracking-widest text-cyan-400/80 font-mono mb-6 uppercase">
          Loading Equirectangular Environment... {{ loadProgress }}%
        </p>
        <div class="w-64 h-1.5 bg-gray-800 rounded-full overflow-hidden border border-cyan-500/30">
          <div
            class="h-full bg-gradient-to-r from-cyan-500 via-fuchsia-500 to-emerald-400 transition-all duration-300"
            :style="{ width: loadProgress + '%' }"
          ></div>
        </div>
      </div>
    </Transition>

    <!-- Top UI Header Overlay -->
    <header class="absolute top-0 inset-x-0 z-30 p-6 flex justify-between items-start pointer-events-none">
      <div class="flex items-center gap-4 bg-gray-900/80 backdrop-blur-md border border-cyan-500/30 px-5 py-3 rounded-2xl shadow-2xl pointer-events-auto">
        <div class="w-3 h-3 rounded-full bg-cyan-400 animate-ping"></div>
        <div>
          <h1 class="font-bold text-base md:text-lg tracking-wider text-white flex items-center gap-2">
            <span>NEO-KYOTO 2099</span>
            <span class="text-[10px] px-2 py-0.5 rounded bg-cyan-500/20 text-cyan-300 border border-cyan-500/40 uppercase font-mono">VR-360 Live</span>
          </h1>
          <p class="text-xs text-gray-400 font-mono flex items-center gap-2">
            <span>HEADING: <strong class="text-cyan-400 font-semibold">{{ currentHeading }}° {{ compassDirection }}</strong></span>
            <span>•</span>
            <span>FOV: <strong class="text-fuchsia-400 font-semibold">{{ currentFov.toFixed(0) }}°</strong></span>
          </p>
        </div>
      </div>

      <!-- Action Badges / Top Right -->
      <div class="flex items-center gap-3 pointer-events-auto">
        <button
          @click="toggleAudio"
          :class="[
            'p-3 rounded-2xl backdrop-blur-md border transition-all duration-300 flex items-center gap-2 text-xs font-mono tracking-wider',
            isAudioPlaying ? 'bg-cyan-500/20 border-cyan-400 text-cyan-300 shadow-lg shadow-cyan-500/20' : 'bg-gray-900/80 border-gray-700 text-gray-400 hover:text-white'
          ]"
        >
          <Volume2 v-if="isAudioPlaying" class="w-4 h-4 text-cyan-400 animate-pulse" />
          <VolumeX v-else class="w-4 h-4" />
          <span class="hidden sm:inline">{{ isAudioPlaying ? 'AUDIO ACTIVE' : 'MUTED' }}</span>
        </button>

        <button
          @click="resetView"
          class="p-3 bg-gray-900/80 hover:bg-gray-800 backdrop-blur-md border border-gray-700 hover:border-cyan-500/50 text-gray-300 hover:text-white rounded-2xl transition-all"
          title="Reset View"
        >
          <RotateCcw class="w-4 h-4" />
        </button>
      </div>
    </header>

    <!-- Interactive Hotspot DOM Markers (Positioned over 3D sphere) -->
    <div v-if="showHotspots && !isLoading" class="absolute inset-0 z-20 pointer-events-none overflow-hidden">
      <div
        v-for="spot in hotspots"
        :key="spot.id"
        v-show="spot.visible"
        :style="{ transform: `translate3d(${spot.screenX}px, ${spot.screenY}px, 0px)` }"
        class="absolute -translate-x-1/2 -translate-y-1/2 pointer-events-auto group cursor-pointer"
        @click="activeHotspot = spot"
      >
        <div class="relative flex items-center justify-center">
          <div class="w-10 h-10 rounded-full bg-cyan-500/20 border-2 border-cyan-400 animate-ping absolute"></div>
          <div class="w-8 h-8 rounded-full bg-cyan-900/90 border border-cyan-300 shadow-lg flex items-center justify-center text-cyan-300 group-hover:scale-125 transition-transform duration-300">
            <Info class="w-4 h-4" />
          </div>
          <!-- Label Tag -->
          <div class="absolute left-10 top-1/2 -translate-y-1/2 bg-gray-950/90 backdrop-blur-md border border-cyan-500/40 text-cyan-300 text-xs px-3 py-1.5 rounded-xl whitespace-nowrap shadow-xl opacity-80 group-hover:opacity-100 group-hover:translate-x-1 transition-all">
            {{ spot.title }}
          </div>
        </div>
      </div>
    </div>

    <!-- Hotspot Details Modal -->
    <Transition
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="opacity-0 scale-95"
      enter-to-class="opacity-100 scale-100"
      leave-active-class="transition duration-200 ease-in"
      leave-from-class="opacity-100 scale-100"
      leave-to-class="opacity-0 scale-95"
    >
      <div
        v-if="activeHotspot"
        class="fixed inset-0 z-40 bg-black/60 backdrop-blur-sm flex items-center justify-center p-6"
        @click.self="activeHotspot = null"
      >
        <div class="bg-gray-900/95 border border-cyan-500/40 text-white rounded-3xl max-w-lg w-full p-6 shadow-2xl relative">
          <button
            @click="activeHotspot = null"
            class="absolute top-5 right-5 text-gray-400 hover:text-white p-1 rounded-full hover:bg-gray-800"
          >
            <X class="w-5 h-5" />
          </button>
          <div class="flex items-center gap-3 mb-4">
            <div class="w-10 h-10 rounded-2xl bg-cyan-500/20 border border-cyan-400 flex items-center justify-center text-cyan-300">
              <Sparkles class="w-5 h-5" />
            </div>
            <div>
              <span class="text-[10px] font-mono tracking-widest text-cyan-400 uppercase">Sector Highlight</span>
              <h3 class="font-serif text-2xl font-bold text-white">{{ activeHotspot.title }}</h3>
            </div>
          </div>
          <p class="text-gray-300 text-sm leading-relaxed mb-6 font-sans">
            {{ activeHotspot.description }}
          </p>
          <div class="flex justify-between items-center pt-4 border-t border-gray-800 text-xs font-mono text-cyan-400/80">
            <span>COORDINATES: {{ activeHotspot.coords.x }}, {{ activeHotspot.coords.y }}, {{ activeHotspot.coords.z }}</span>
            <button
              @click="activeHotspot = null"
              class="px-4 py-2 bg-cyan-500/20 hover:bg-cyan-500/30 text-cyan-300 border border-cyan-500/40 rounded-xl transition-colors font-sans font-medium text-xs"
            >
              Close Info
            </button>
          </div>
        </div>
      </div>
    </Transition>

    <!-- Bottom Floating Toolbar -->
    <footer class="absolute bottom-6 inset-x-0 z-30 flex justify-center pointer-events-none px-6">
      <div class="bg-gray-900/80 backdrop-blur-md border border-cyan-500/30 px-6 py-3 rounded-full shadow-2xl flex items-center gap-3 md:gap-5 pointer-events-auto">
        <!-- Auto Rotate Toggle -->
        <button
          @click="toggleAutoRotate"
          :class="[
            'p-2.5 rounded-full transition-all flex items-center gap-2 text-xs font-medium',
            isAutoRotate ? 'bg-cyan-500 text-gray-950 font-bold shadow-lg shadow-cyan-500/30' : 'text-gray-300 hover:text-white hover:bg-gray-800'
          ]"
          title="Toggle 360 Auto Rotation"
        >
          <Play v-if="!isAutoRotate" class="w-4 h-4" />
          <Pause v-else class="w-4 h-4" />
          <span class="hidden sm:inline">{{ isAutoRotate ? 'Auto Rotate: ON' : 'Auto Rotate' }}</span>
        </button>

        <div class="w-px h-6 bg-gray-700"></div>

        <!-- Zoom Controls -->
        <button
          @click="zoomIn"
          class="p-2.5 text-gray-300 hover:text-cyan-400 hover:bg-gray-800 rounded-full transition-colors"
          title="Zoom In"
        >
          <ZoomIn class="w-4 h-4" />
        </button>
        <button
          @click="zoomOut"
          class="p-2.5 text-gray-300 hover:text-cyan-400 hover:bg-gray-800 rounded-full transition-colors"
          title="Zoom Out"
        >
          <ZoomOut class="w-4 h-4" />
        </button>

        <div class="w-px h-6 bg-gray-700"></div>

        <!-- Toggle Hotspots -->
        <button
          @click="showHotspots = !showHotspots"
          :class="[
            'p-2.5 rounded-full transition-colors text-xs font-medium flex items-center gap-1.5',
            showHotspots ? 'text-cyan-400 bg-cyan-500/10 border border-cyan-500/30' : 'text-gray-400 hover:text-white'
          ]"
          title="Toggle Hotspots"
        >
          <Eye class="w-4 h-4" />
          <span class="hidden md:inline">Hotspots</span>
        </button>

        <!-- Fullscreen Toggle -->
        <button
          @click="toggleFullscreen"
          class="p-2.5 text-gray-300 hover:text-cyan-400 hover:bg-gray-800 rounded-full transition-colors"
          title="Toggle Fullscreen Mode"
        >
          <Maximize class="w-4 h-4" />
        </button>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import * as THREE from 'three'
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
  Maximize
} from 'lucide-vue-next'

const canvasContainer = ref<HTMLDivElement | null>(null)
const isLoading = ref(true)
const loadProgress = ref(0)
const isAutoRotate = ref(true)
const isAudioPlaying = ref(false)
const showHotspots = ref(true)
const currentFov = ref(75)
const currentHeading = ref(0)

interface Hotspot {
  id: number
  title: string
  description: string
  coords: { x: number; y: number; z: number }
  screenX: number
  screenY: number
  visible: boolean
}

const activeHotspot = ref<Hotspot | null>(null)

const hotspots = ref<Hotspot[]>([
  {
    id: 1,
    title: 'NEO-KYOTO Central Citadel',
    description: 'The monumental core of Neo-Kyoto featuring high-density vertical architecture, energy conduits, and mega-holographic displays.',
    coords: { x: 0, y: 100, z: -400 },
    screenX: 0,
    screenY: 0,
    visible: false
  },
  {
    id: 2,
    title: 'Elevated Monorail Network',
    description: 'Autonomous maglev sky-trains operating at 300 km/h, interconnecting upper urban strata and industrial hubs across the metropolis.',
    coords: { x: -350, y: -20, z: -250 },
    screenX: 0,
    screenY: 0,
    visible: false
  },
  {
    id: 3,
    title: 'Cyber Ramen & Noodle Lounge',
    description: 'A vibrant street-level eatery illuminated by authentic neon signage, serving hot ramen broth to synth-workers and travelers.',
    coords: { x: 300, y: -120, z: -280 },
    screenX: 0,
    screenY: 0,
    visible: false
  },
  {
    id: 4,
    title: 'VR Viewport Promenade',
    description: 'Observation balcony overlooking the multi-level traffic decks, offering panoramic sunset vistas over the futuristic skyline.',
    coords: { x: 150, y: -160, z: 350 },
    screenX: 0,
    screenY: 0,
    visible: false
  }
])

// Three.js Variables
let scene: THREE.Scene
let camera: THREE.PerspectiveCamera
let renderer: THREE.WebGLRenderer
let sphereMesh: THREE.Mesh
let animationFrameId: number

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

const compassDirection = computed(() => {
  const h = currentHeading.value
  if (h >= 337.5 || h < 22.5) return 'N'
  if (h >= 22.5 && h < 67.5) return 'NE'
  if (h >= 67.5 && h < 112.5) return 'E'
  if (h >= 112.5 && h < 157.5) return 'SE'
  if (h >= 157.5 && h < 202.5) return 'S'
  if (h >= 202.5 && h < 247.5) return 'SW'
  if (h >= 247.5 && h < 292.5) return 'W'
  return 'NW'
})

const initVR360 = () => {
  if (!canvasContainer.value) return

  const width = window.innerWidth
  const height = window.innerHeight

  // 1. Scene
  scene = new THREE.Scene()

  // 2. Camera
  camera = new THREE.PerspectiveCamera(75, width / height, 1, 1100)
  camera.target = new THREE.Vector3(0, 0, 0)

  // 3. Geometry & Texture mapping for 360 panorama
  const geometry = new THREE.SphereGeometry(500, 60, 40)
  geometry.scale(-1, 1, 1) // Invert geometry so texture faces inward

  const textureLoader = new THREE.TextureLoader()
  textureLoader.load(
    '/image/paranoma.png',
    (texture) => {
      texture.colorSpace = THREE.SRGBColorSpace
      const material = new THREE.MeshBasicMaterial({ map: texture })
      sphereMesh = new THREE.Mesh(geometry, material)
      scene.add(sphereMesh)

      isLoading.value = false
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
      isLoading.value = false
    }
  )

  // 4. Renderer
  renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false })
  renderer.setPixelRatio(window.devicePixelRatio)
  renderer.setSize(width, height)
  canvasContainer.value.appendChild(renderer.domElement)

  // Event Listeners
  const domElement = renderer.domElement
  domElement.addEventListener('pointerdown', onPointerDown)
  domElement.addEventListener('wheel', onDocumentMouseWheel, { passive: false })
  window.addEventListener('resize', onWindowResize)

  // Start Animation Loop
  animate()
}

const onPointerDown = (event: PointerEvent) => {
  if (event.isPrimary === false) return

  isUserInteracting = true

  onPointerDownPointerX = event.clientX
  onPointerDownPointerY = event.clientY

  onPointerDownLon = lon
  onPointerDownLat = lat

  document.addEventListener('pointermove', onPointerMove)
  document.addEventListener('pointerup', onPointerUp)
}

const onPointerMove = (event: PointerEvent) => {
  if (event.isPrimary === false) return

  lon = (onPointerDownPointerX - event.clientX) * 0.1 + onPointerDownLon
  lat = (event.clientY - onPointerDownPointerY) * 0.1 + onPointerDownLat
}

const onPointerUp = (event: PointerEvent) => {
  if (event.isPrimary === false) return

  isUserInteracting = false

  document.removeEventListener('pointermove', onPointerMove)
  document.removeEventListener('pointerup', onPointerUp)
}

const onDocumentMouseWheel = (event: WheelEvent) => {
  event.preventDefault()
  const fov = camera.fov + event.deltaY * 0.05
  camera.fov = THREE.MathUtils.clamp(fov, 30, 100)
  camera.updateProjectionMatrix()
  currentFov.value = camera.fov
}

const onWindowResize = () => {
  if (!camera || !renderer) return
  camera.aspect = window.innerWidth / window.innerHeight
  camera.updateProjectionMatrix()
  renderer.setSize(window.innerWidth, window.innerHeight)
}

const updateHotspots = () => {
  if (!camera || !renderer) return

  const halfWidth = window.innerWidth / 2
  const halfHeight = window.innerHeight / 2

  hotspots.value.forEach((spot) => {
    const vector = new THREE.Vector3(spot.coords.x, spot.coords.y, spot.coords.z)

    // Check if behind camera
    const cameraDir = new THREE.Vector3()
    camera.getWorldDirection(cameraDir)

    const spotDir = vector.clone().sub(camera.position).normalize()
    const dot = cameraDir.dot(spotDir)

    if (dot > 0.2) {
      vector.project(camera)
      spot.screenX = vector.x * halfWidth + halfWidth
      spot.screenY = -(vector.y * halfHeight) + halfHeight
      spot.visible = true
    } else {
      spot.visible = false
    }
  })
}

const animate = () => {
  animationFrameId = requestAnimationFrame(animate)

  if (isAutoRotate.value && !isUserInteracting) {
    lon += 0.08
  }

  lat = Math.max(-85, Math.min(85, lat))
  phi = THREE.MathUtils.degToRad(90 - lat)
  theta = THREE.MathUtils.degToRad(lon)

  const x = 500 * Math.sin(phi) * Math.cos(theta)
  const y = 500 * Math.cos(phi)
  const z = 500 * Math.sin(phi) * Math.sin(theta)

  camera.lookAt(x, y, z)

  // Update Compass Heading
  let heading = Math.round((lon % 360 + 360) % 360)
  currentHeading.value = heading

  updateHotspots()

  renderer.render(scene, camera)
}

const resetView = () => {
  lon = 0
  lat = 0
  camera.fov = 75
  camera.updateProjectionMatrix()
  currentFov.value = 75
}

const toggleAutoRotate = () => {
  isAutoRotate.value = !isAutoRotate.value
}

const zoomIn = () => {
  camera.fov = Math.max(30, camera.fov - 10)
  camera.updateProjectionMatrix()
  currentFov.value = camera.fov
}

const zoomOut = () => {
  camera.fov = Math.min(100, camera.fov + 10)
  camera.updateProjectionMatrix()
  currentFov.value = camera.fov
}

const toggleFullscreen = () => {
  if (!document.fullscreenElement) {
    document.documentElement.requestFullscreen()
  } else {
    document.exitFullscreen()
  }
}

// Audio Ambient sound synth synthesizer using Web Audio API
let audioCtx: AudioContext | null = null
let osc1: OscillatorNode | null = null
let osc2: OscillatorNode | null = null

const toggleAudio = () => {
  if (!isAudioPlaying.value) {
    try {
      audioCtx = new (window.AudioContext || (window as any).webkitAudioContext)()

      osc1 = audioCtx.createOscillator()
      osc2 = audioCtx.createOscillator()
      const gain = audioCtx.createGain()

      osc1.type = 'sine'
      osc1.frequency.setValueAtTime(110, audioCtx.currentTime) // A2 note

      osc2.type = 'triangle'
      osc2.frequency.setValueAtTime(164.81, audioCtx.currentTime) // E3 note

      gain.gain.setValueAtTime(0.05, audioCtx.currentTime)

      osc1.connect(gain)
      osc2.connect(gain)
      gain.connect(audioCtx.destination)

      osc1.start()
      osc2.start()

      isAudioPlaying.value = true
    } catch (e) {
      console.error('Audio initialization failed', e)
    }
  } else {
    if (osc1) osc1.stop()
    if (osc2) osc2.stop()
    if (audioCtx) audioCtx.close()
    isAudioPlaying.value = false
  }
}

onMounted(() => {
  initVR360()
})

onUnmounted(() => {
  cancelAnimationFrame(animationFrameId)
  if (renderer && renderer.domElement) {
    renderer.domElement.remove()
  }
  if (osc1) osc1.stop()
  if (osc2) osc2.stop()
  if (audioCtx) audioCtx.close()
})
</script>