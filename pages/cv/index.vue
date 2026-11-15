<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
import { RoomEnvironment } from 'three/addons/environments/RoomEnvironment.js';

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

    // 5. Vật liệu
    const goldMaterial = new THREE.MeshPhysicalMaterial({
        color: 0xffd700,
        emissive: 0x332200,
        emissiveIntensity: 0.25,
        metalness: 0.92,
        roughness: 0.16,
        clearcoat: 0.4,
        clearcoatRoughness: 0.12,
        reflectivity: 1.0,
    });

    const brightGoldMaterial = new THREE.MeshPhysicalMaterial({
        color: 0xffe44d,
        emissive: 0x443000,
        emissiveIntensity: 0.3,
        metalness: 0.95,
        roughness: 0.12,
        clearcoat: 0.5,
    });

    const blackBaseMaterial = new THREE.MeshPhysicalMaterial({
        color: 0x111114,
        metalness: 0.25,
        roughness: 0.14,
        clearcoat: 0.9,
        clearcoatRoughness: 0.08,
        reflectivity: 0.9,
    });

    // 6. Dựng chiếc cúp
    trophyGroup = new THREE.Group();
    scene.add(trophyGroup);

    // Hàm tạo trụ bo cạnh bằng LatheGeometry
    const createBeveledCylinder = (radius, height, bevel = 0.045, radialSegments = 64) => {
        const r = radius;
        const h = height;
        const b = Math.min(bevel, r * 0.3, h * 0.3);
        const points = [
            new THREE.Vector2(0, 0),
            new THREE.Vector2(r - b, 0),
            new THREE.Vector2(r, b),
            new THREE.Vector2(r, h - b),
            new THREE.Vector2(r - b, h),
            new THREE.Vector2(0, h),
        ];
        const geo = new THREE.LatheGeometry(points, radialSegments);
        geo.computeVertexNormals();
        return geo;
    };

    // Tầng đế 1 (Dưới cùng - Đen bóng, nhỏ gọn)
    const tier1Radius = 0.95;
    const tier1Height = 0.26;
    const tier1Geo = createBeveledCylinder(tier1Radius, tier1Height, 0.045);
    const tier1Mesh = new THREE.Mesh(tier1Geo, blackBaseMaterial);
    tier1Mesh.castShadow = true;
    tier1Mesh.receiveShadow = true;
    trophyGroup.add(tier1Mesh);

    // Viền chỉ vàng quanh đế 1
    const trimRing1Geo = new THREE.TorusGeometry(tier1Radius + 0.004, 0.012, 16, 64);
    trimRing1Geo.rotateX(Math.PI / 2);
    const trimRing1 = new THREE.Mesh(trimRing1Geo, brightGoldMaterial);
    trimRing1.position.y = tier1Height * 0.5;
    trophyGroup.add(trimRing1);

    // Tầng đế 2 (Tầng trên - Vàng 24K)
    const tier2Radius = 0.58;
    const tier2Height = 0.32;
    const tier2Geo = createBeveledCylinder(tier2Radius, tier2Height, 0.04);
    const tier2Mesh = new THREE.Mesh(tier2Geo, goldMaterial);
    tier2Mesh.position.y = tier1Height;
    tier2Mesh.castShadow = true;
    tier2Mesh.receiveShadow = true;
    trophyGroup.add(tier2Mesh);

    // Vòng khuyên vàng tại cổ tiếp giáp thân cúp
    const collarY = tier1Height + tier2Height;
    const collarGeo = new THREE.TorusGeometry(0.22, 0.028, 20, 48);
    collarGeo.rotateX(Math.PI / 2);
    const collarMesh = new THREE.Mesh(collarGeo, brightGoldMaterial);
    collarMesh.position.y = collarY + 0.01;
    trophyGroup.add(collarMesh);

    // Thân chính đứng thẳng vát chéo ở đỉnh
    const stemWidth = 0.28;
    const stemDepth = 0.16;
    const stemHeight = 2.05;
    const bevelTop = 0.24;

    const stemShape = new THREE.Shape();
    stemShape.moveTo(-stemWidth / 2, 0);
    stemShape.lineTo(stemWidth / 2, 0);
    stemShape.lineTo(stemWidth / 2, stemHeight);
    stemShape.lineTo(-stemWidth / 2, stemHeight - bevelTop);
    stemShape.closePath();

    const stemExtrude = new THREE.ExtrudeGeometry(stemShape, {
        depth: stemDepth,
        bevelEnabled: true,
        bevelThickness: 0.025,
        bevelSize: 0.022,
        bevelSegments: 5,
    });
    stemExtrude.translate(0, 0, -stemDepth / 2);

    const stemMesh = new THREE.Mesh(stemExtrude, goldMaterial);
    stemMesh.position.y = collarY - 0.02;
    stemMesh.castShadow = true;
    stemMesh.receiveShadow = true;
    trophyGroup.add(stemMesh);

    // Nhánh lá uốn cong bên phải
    const leafGroup = new THREE.Group();
    leafGroup.position.set(stemWidth * 0.32, collarY + 0.65, 0);

    const leafShape = new THREE.Shape();
    leafShape.moveTo(0, 0);
    leafShape.quadraticCurveTo(0.45, 0.12, 0.78, 0.68);
    leafShape.quadraticCurveTo(0.28, 0.56, 0, 0.22);
    leafShape.closePath();

    const leafDepth = 0.07;
    const leafGeo = new THREE.ExtrudeGeometry(leafShape, {
        depth: leafDepth,
        bevelEnabled: true,
        bevelThickness: 0.02,
        bevelSize: 0.018,
        bevelSegments: 5,
    });
    leafGeo.translate(0, 0, -leafDepth / 2);

    const leafMesh = new THREE.Mesh(leafGeo, brightGoldMaterial);
    leafMesh.castShadow = true;
    leafMesh.receiveShadow = true;
    leafGroup.add(leafMesh);

    // Gân lá tạo khối 3D
    const veinCurve = new THREE.QuadraticBezierCurve3(
        new THREE.Vector3(0.02, 0.1, 0),
        new THREE.Vector3(0.36, 0.32, 0.02),
        new THREE.Vector3(0.72, 0.63, 0)
    );
    const veinGeo = new THREE.TubeGeometry(veinCurve, 24, 0.014, 8, false);
    const veinMesh = new THREE.Mesh(veinGeo, goldMaterial);
    leafGroup.add(veinMesh);

    trophyGroup.add(leafGroup);

    // Căn giữa mô hình và góc nhìn mặc định
    trophyGroup.position.y = -1.25;
    trophyGroup.rotation.y = -0.35;

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

<style scoped>
.trophy-wrapper {
    position: relative;
    width: 100%;
    height: 100vh;
    overflow: hidden;
    background: radial-gradient(circle at 50% 42%, #9e141b 0%, #5e080d 55%, #2b0204 100%);
    font-family: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
    user-select: none;
}

.canvas-container {
    width: 100%;
    height: 100%;
    display: block;
}

.trophy-header {
    position: absolute;
    top: 24px;
    left: 0;
    right: 0;
    text-align: center;
    pointer-events: none;
    z-index: 10;
}

.subtitle {
    margin: 0;
    font-size: 11px;
    letter-spacing: 0.25em;
    color: rgba(255, 224, 130, 0.8);
    font-weight: 500;
}

.title {
    margin: 6px 0 0;
    font-size: clamp(20px, 4vw, 30px);
    font-weight: 700;
    letter-spacing: 0.1em;
    background: linear-gradient(135deg, #fff3b0 0%, #ffd700 45%, #e6ac00 100%);
    -webkit-text-fill-color: transparent;
    filter: drop-shadow(0 2px 10px rgba(0, 0, 0, 0.45));
}

.trophy-controls {
    position: absolute;
    bottom: 24px;
    left: 0;
    right: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
    z-index: 10;
    padding: 0 16px;
}

.control-panel {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    gap: 14px;
    background: rgba(35, 3, 5, 0.65);
    backdrop-filter: blur(12px);
    border: 1px solid rgba(255, 215, 0, 0.28);
    padding: 10px 18px;
    border-radius: 16px;
    box-shadow: 0 12px 32px rgba(0, 0, 0, 0.45);
}

.btn-toggle {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 6px 14px;
    border-radius: 999px;
    border: 1px solid rgba(255, 215, 0, 0.4);
    background: rgba(255, 215, 0, 0.15);
    color: #fff3b0;
    font-size: 13px;
    font-weight: 500;
    cursor: pointer;
    transition: background 0.2s;
}

.btn-toggle:hover {
    background: rgba(255, 215, 0, 0.28);
}

.status-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background-color: #ffd700;
    transition: background-color 0.2s;
}

.status-dot.paused {
    background-color: rgba(255, 255, 255, 0.4);
}

.speed-group {
    display: flex;
    align-items: center;
    gap: 8px;
    color: rgba(255, 243, 176, 0.85);
    font-size: 13px;
}

.speed-group input[type='range'] {
    width: 96px;
    accent-color: #ffd700;
    cursor: pointer;
}

.btn-reset {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 6px 10px;
    border-radius: 999px;
    border: 1px solid rgba(255, 215, 0, 0.35);
    background: rgba(255, 255, 255, 0.08);
    color: #fff3b0;
    cursor: pointer;
    transition: background 0.2s;
}

.btn-reset:hover {
    background: rgba(255, 255, 255, 0.2);
}

.hint-text {
    margin: 0;
    font-size: 12px;
    color: rgba(255, 236, 179, 0.65);
    text-align: center;
}
</style>