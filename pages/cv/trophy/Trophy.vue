<script>
import * as THREE from 'three';
import { goldMaterial, brightGoldMaterial, blackBaseMaterial } from '../Material.vue';

export const createTrophy = (scene) => {
    const trophyGroup = new THREE.Group();
    scene.add(trophyGroup);

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

    const tier1Radius = 0.95;
    const tier1Height = 0.26;
    const tier1Geo = createBeveledCylinder(tier1Radius, tier1Height, 0.045);
    const tier1Mesh = new THREE.Mesh(tier1Geo, blackBaseMaterial);
    tier1Mesh.castShadow = true;
    tier1Mesh.receiveShadow = true;
    trophyGroup.add(tier1Mesh);

    const trimRing1Geo = new THREE.TorusGeometry(tier1Radius + 0.004, 0.012, 16, 64);
    trimRing1Geo.rotateX(Math.PI / 2);
    const trimRing1 = new THREE.Mesh(trimRing1Geo, brightGoldMaterial);
    trimRing1.position.y = tier1Height * 0.5;
    trophyGroup.add(trimRing1);

    const tier2Radius = 0.58;
    const tier2Height = 0.32;
    const tier2Geo = createBeveledCylinder(tier2Radius, tier2Height, 0.04);
    const tier2Mesh = new THREE.Mesh(tier2Geo, goldMaterial);
    tier2Mesh.position.y = tier1Height;
    tier2Mesh.castShadow = true;
    tier2Mesh.receiveShadow = true;
    trophyGroup.add(tier2Mesh);

    const collarY = tier1Height + tier2Height;
    const collarGeo = new THREE.TorusGeometry(0.22, 0.028, 20, 48);
    collarGeo.rotateX(Math.PI / 2);
    const collarMesh = new THREE.Mesh(collarGeo, brightGoldMaterial);
    collarMesh.position.y = collarY + 0.01;
    trophyGroup.add(collarMesh);

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

    const veinCurve = new THREE.QuadraticBezierCurve3(
        new THREE.Vector3(0.02, 0.1, 0),
        new THREE.Vector3(0.36, 0.32, 0.02),
        new THREE.Vector3(0.72, 0.63, 0)
    );
    const veinGeo = new THREE.TubeGeometry(veinCurve, 24, 0.014, 8, false);
    const veinMesh = new THREE.Mesh(veinGeo, goldMaterial);
    leafGroup.add(veinMesh);

    trophyGroup.add(leafGroup);
    trophyGroup.position.y = -1.25;
    trophyGroup.rotation.y = -0.35;

    return trophyGroup;
};

export default {
    name: 'Trophy',
};
</script>
