<script>
import * as THREE from 'three';

export const goldMaterial = new THREE.MeshPhysicalMaterial({
    color: 0xffd700,
    emissive: 0x332200,
    emissiveIntensity: 0.25,
    metalness: 0.92,
    roughness: 0.16,
    clearcoat: 0.4,
    clearcoatRoughness: 0.12,
    reflectivity: 1.0,
});

export const brightGoldMaterial = new THREE.MeshPhysicalMaterial({
    color: 0xffe44d,
    emissive: 0x443000,
    emissiveIntensity: 0.3,
    metalness: 0.95,
    roughness: 0.12,
    clearcoat: 0.5,
});

export const blackBaseMaterial = new THREE.MeshPhysicalMaterial({
    color: 0x111114,
    metalness: 0.25,
    roughness: 0.14,
    clearcoat: 0.9,
    clearcoatRoughness: 0.08,
    reflectivity: 0.9,
});

export default {
    name: 'CvMaterials',
};
</script>

<template>
    <span hidden aria-hidden="true"></span>
</template>
