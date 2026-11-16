<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
import { RoomEnvironment } from 'three/addons/environments/RoomEnvironment.js';
import { createTrophy } from './trophy/Trophy.vue';
import './style.css';

const containerRef = ref(null);
const isRotating = ref(true);
const rotationSpeed = ref(0.005);

let scene, camera, renderer, controls, trophyGroup, particles;
let animationFrameId = null;

// Khởi tạo và dựng mô hình 3D
const initThree = () => {
    const container = containerRef.value;
    if (!container) return;

    const width = container.clientWidth || window.innerWidth;
    const height = container.clientHeight || window.innerHeight;

    // 1. Scene & Renderer
    scene = new THREE.Scene();

    renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    renderer.setSize(width, height);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    renderer.toneMapping = THREE.ACESFilmicToneMapping;
    renderer.toneMappingExposure = 1.15;
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFSoftShadowMap;
    container.appendChild(renderer.domElement);

    // 2. Camera
    camera = new THREE.PerspectiveCamera(40, width / height, 0.1, 100);
    camera.position.set(0, 2.3, 6.2);

    // 3. Môi trường phản chiếu studio sáng giúp vàng rực rỡ
    const pmremGenerator = new THREE.PMREMGenerator(renderer);
    const roomEnv = new RoomEnvironment();
    scene.environment = pmremGenerator.fromScene(roomEnv, 0.04).texture;
    scene.environmentIntensity = 0.85;

    // 4. Hệ thống ánh sáng
    const ambientLight = new THREE.AmbientLight(0xfff8e7, 0.6);
    scene.add(ambientLight);

    const keyLight = new THREE.DirectionalLight(0xffffff, 2.2);
    keyLight.position.set(4, 7, 5);
    keyLight.castShadow = true;
    keyLight.shadow.mapSize.set(1024, 1024);
    scene.add(keyLight);

    const fillLight = new THREE.DirectionalLight(0xfff0b3, 1.4);
    fillLight.position.set(-5, 3, 4);
    scene.add(fillLight);

    const rimLight = new THREE.DirectionalLight(0xffe066, 1.6);
    rimLight.position.set(0, 5, -6);
    scene.add(rimLight);

    // 6. Dựng chiếc cúp
    trophyGroup = createTrophy(scene);

    // Sàn hứng bóng mờ
    const shadowPlaneGeo = new THREE.PlaneGeometry(12, 12);
    shadowPlaneGeo.rotateX(-Math.PI / 2);
    const shadowPlaneMat = new THREE.ShadowMaterial({ opacity: 0.35 });
    const shadowPlane = new THREE.Mesh(shadowPlaneGeo, shadowPlaneMat);
    shadowPlane.position.y = -1.26;
    shadowPlane.receiveShadow = true;
    scene.add(shadowPlane);

    // Hạt bụi vàng lơ lửng
    const particleCount = 70;
    const particlePositions = new Float32Array(particleCount * 3);
    for (let i = 0; i < particleCount; i++) {
        particlePositions[i * 3] = (Math.random() - 0.5) * 7;
        particlePositions[i * 3 + 1] = (Math.random() - 0.5) * 5;
        particlePositions[i * 3 + 2] = (Math.random() - 0.5) * 6;
    }
    const particleGeo = new THREE.BufferGeometry();
    particleGeo.setAttribute('position', new THREE.BufferAttribute(particlePositions, 3));
    const particleMat = new THREE.PointsMaterial({
        color: 0xffe066,
        size: 0.035,
        transparent: true,
        opacity: 0.55,
    });
    particles = new THREE.Points(particleGeo, particleMat);
    scene.add(particles);

    // 7. OrbitControls
    controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.dampingFactor = 0.06;
    controls.minDistance = 3.2;
    controls.maxDistance = 11;
    controls.maxPolarAngle = Math.PI * 0.55;
    controls.target.set(0, 0.15, 0);
    controls.update();

    // 8. Vòng lặp render
    const animate = () => {
        animationFrameId = requestAnimationFrame(animate);

        if (isRotating.value && trophyGroup) {
            trophyGroup.rotation.y += Number(rotationSpeed.value);
        }

        if (particles) {
            particles.rotation.y -= 0.0008;
        }

        controls.update();
        renderer.render(scene, camera);
    };
    animate();
};

// Xử lý resize
const handleResize = () => {
    const container = containerRef.value;
    if (!container || !camera || !renderer) return;

    const width = container.clientWidth || window.innerWidth;
    const height = container.clientHeight || window.innerHeight;
    camera.aspect = width / height;
    camera.updateProjectionMatrix();
    renderer.setSize(width, height);
};

// Các hàm điều khiển UI
const toggleRotation = () => {
    isRotating.value = !isRotating.value;
};

const resetCamera = () => {
    camera.position.set(0, 2.3, 6.2);
    controls.target.set(0, 0.15, 0);
    controls.update();
    if (trophyGroup) {
        trophyGroup.rotation.y = -0.35;
    }
};

onMounted(() => {
    initThree();
    window.addEventListener('resize', handleResize);
});

onBeforeUnmount(() => {
    window.removeEventListener('resize', handleResize);
    if (animationFrameId) {
        cancelAnimationFrame(animationFrameId);
    }
    if (renderer) {
        renderer.dispose();
    }
});
</script>

<template>
    <div class="trophy-wrapper">
        <!-- Khung chứa Canvas 3D -->
        <div ref="containerRef" class="canvas-container"></div>

        <!-- Tiêu đề phía trên -->
        <header class="trophy-header">
            <p class="subtitle">MÔ HÌNH 3D TƯƠNG TÁC</p>
            <h1 class="title">CÚP VÀNG VINH QUANG</h1>
        </header>

        <!-- Thanh điều khiển phía dưới -->
        <footer class="trophy-controls">
            <div class="control-panel">
                <button class="btn-toggle" @click="toggleRotation">
                    <span class="status-dot" :class="{ paused: !isRotating }"></span>
                    <span>{{ isRotating ? 'Đang xoay' : 'Tạm dừng' }}</span>
                </button>

                <div class="speed-group">
                    <label for="speedSlider">Tốc độ</label>
                    <input id="speedSlider" v-model.number="rotationSpeed" type="range" min="0.001" max="0.025"
                        step="0.001" />
                </div>

                <button class="btn-reset" title="Đặt lại góc nhìn" @click="resetCamera">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"
                        stroke-linecap="round" stroke-linejoin="round">
                        <path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8" />
                        <path d="M3 3v5h5" />
                    </svg>
                </button>
            </div>
            <p class="hint-text">Kéo để xoay 360° • Cuộn hoặc chụm ngón tay để phóng to</p>
        </footer>
    </div>
</template>