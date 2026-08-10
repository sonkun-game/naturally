<template>
    <div class="my-three-js">
    </div>
</template>


<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import * as THREE from 'three'
import Stats from 'three/addons/libs/stats.module.js'
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'


onMounted(async () => {

    const scene = new THREE.Scene()
    scene.environment = new THREE.CubeTextureLoader().setPath('https://sbcode.net/img/').load(['px.png', 'nx.png', 'py.png', 'ny.png', 'pz.png', 'nz.png'])

    const gridHelper = new THREE.GridHelper()
    scene.add(gridHelper)


    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
    camera.position.set(10, 10, 10)
    camera.lookAt(0, 0, 0)

    const renderer = new THREE.WebGLRenderer()
    renderer.setSize(window.innerWidth, window.innerHeight)
    document.body.appendChild(renderer.domElement)

    const controls = new OrbitControls(camera, renderer.domElement)
    // controls.target.set(8, 0, 0)
    // controls.update()

    // Light
    const ambientLight = new THREE.AmbientLight(0xffffff, Math.PI);
    ambientLight.intensity = 1.5
    ambientLight.visible = true;
    scene.add(ambientLight);

    // Material
    const snowMaterial = new THREE.MeshStandardMaterial({
        color: 0xffffff,
        roughness: 0.9,
        metalness: 0.0
    })

    // Body ============================================>
    const object1 = new THREE.Mesh(new THREE.SphereGeometry(), snowMaterial)
    object1.scale.set(1.5, 1.5, 1.5)
    object1.position.set(0, 0.5, 0)
    scene.add(object1)

    const object2 = new THREE.Mesh(new THREE.SphereGeometry(), snowMaterial)
    object2.scale.set(1.2, 1.2, 1.2)
    object2.position.set(0, 2, 0)
    scene.add(object2)

    const object3 = new THREE.Mesh(new THREE.SphereGeometry(), snowMaterial)
    object3.scale.set(1, 1, 1)
    object3.position.set(0, 3.5, 0)
    scene.add(object3)

    // Eye ================================================>
    const eyeMaterial = new THREE.MeshStandardMaterial({
        color: 0x000000,
        roughness: 0.5
    })

    const eyeGeometry = new THREE.SphereGeometry(0.15, 32, 32)

    // Left eye
    const leftEye = new THREE.Mesh(eyeGeometry, eyeMaterial)
    leftEye.position.set(0.8, 3.8, 0.4)

    // Right eye
    const rightEye = new THREE.Mesh(eyeGeometry, eyeMaterial)
    rightEye.position.set(0.8, 3.8, -0.4)

    scene.add(leftEye)
    scene.add(rightEye)

    // Nose ====================================================>
    const carrotMaterial = new THREE.MeshStandardMaterial({
        color: 0xff6600,
        roughness: 0.8,
        metalness: 0
    })

    const carrotGeometry = new THREE.ConeGeometry(
        0.18,
        0.8,
        32
    )

    const carrotNose = new THREE.Mesh(
        carrotGeometry,
        carrotMaterial
    )

    carrotNose.rotation.x = -Math.PI / 2
    carrotNose.rotation.y = -Math.PI
    carrotNose.rotation.z = Math.PI / 2

    carrotNose.position.set(1, 3.6, 0)
    scene.add(carrotNose)


    // Event ================================================>
    window.addEventListener('resize', () => {
        camera.aspect = window.innerWidth / window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
    })

    const grassMaterial = new THREE.MeshStandardMaterial({
        color: 0x3f8f3f,
        roughness: 0.9,
        metalness: 0.0
    })

    const plane = new THREE.Mesh(new THREE.PlaneGeometry(10, 10), grassMaterial)
    plane.rotation.x = -Math.PI / 2;
    scene.add(plane)

    const stats = new Stats()
    document.body.appendChild(stats.dom)

    function animate() {
        requestAnimationFrame(animate)
        renderer.render(scene, camera)
        stats.update()
    }

    animate()
})

</script>

<style scoped></style>