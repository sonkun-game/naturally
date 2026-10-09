# Paranoma component

`Paranoma.vue` là trình xem panorama 360° dùng Three.js. Component phụ trách render cảnh, điều khiển góc nhìn, hotspot, âm thanh và chuyển cảnh. Trang cha cung cấp danh sách cảnh và lắng nghe các sự kiện nếu cần đồng bộ trạng thái.

## Cài đặt

Đặt component tại `components/paranoma/Paranoma.vue`. Nuxt tự động nhận diện component theo tên `Paranoma`; trang hiện tại cũng có thể import trực tiếp:

```ts
import Paranoma from '~/components/paranoma/Paranoma.vue'
```

## Đầu vào

| Prop | Kiểu | Bắt buộc | Mô tả |
| --- | --- | --- | --- |
| `scenes` | `Partial<Record<SceneKey, PanoramaScene>>` | Có | Các cảnh panorama, bao gồm tên hiển thị, danh mục, đường dẫn ảnh và hotspot. `SceneKey` hiện gồm `outdoor`, `cyber_mall`, `inside_bar`, `ocean`, `shipwreck` và `coral`. |
| `initialSceneId` | `SceneKey` | Có | Khóa cảnh mở đầu; phải tồn tại trong `scenes`. |

Mỗi hotspot cần `id`, `title`, `type` (`info` hoặc `teleport`) và tọa độ 3D `coords: { x, y, z }`. Hotspot `info` nên có `description`; hotspot `teleport` cần `targetScene` khớp với một khóa trong `scenes`. `screenX`, `screenY`, `visible` là trạng thái chiếu lên màn hình do component cập nhật.

Ảnh panorama nên là ảnh equirectangular 360°; `texture` nhận đường dẫn public của Nuxt, ví dụ `/image/paranoma.png`.

## Đầu ra

| Sự kiện | Payload | Khi nào phát |
| --- | --- | --- |
| `scene-change` | `sceneId: string` | Khi chuyển cảnh hoàn tất bước đổi cảnh. |
| `heading-change` | `heading: number` | Trong vòng lặp render, theo hướng nhìn hiện tại (độ). |
| `fov-change` | `fov: number` | Khi FOV thay đổi do zoom, reset hoặc chuyển cảnh. |
| `hotspot-select` | `Hotspot` | Khi người dùng nhấn một hotspot, trước khi mở thông tin hoặc chuyển cảnh. |

## Ví dụ

```vue
<template>
  <Paranoma
    :scenes="scenes"
    initial-scene-id="outdoor"
    @scene-change="onSceneChange"
    @heading-change="heading = $event"
    @fov-change="fov = $event"
    @hotspot-select="onHotspotSelect"
  />
</template>

<script setup lang="ts">
import Paranoma from '~/components/paranoma/Paranoma.vue'

const scenes = {
  outdoor: {
    name: 'City Skyline',
    category: 'OUTDOOR',
    texture: '/image/city-360.jpg',
    hotspots: [
      {
        id: 1,
        title: 'Enter the mall',
        type: 'teleport',
        targetScene: 'mall',
        coords: { x: -220, y: -40, z: -350 },
        screenX: 0,
        screenY: 0,
        visible: false,
      },
    ],
  },
  mall: {
    name: 'Cyber Mall',
    category: 'INDOOR',
    texture: '/image/mall-360.jpg',
    hotspots: [],
  },
}

const heading = ref(0)
const fov = ref(75)
const onSceneChange = (sceneId: string) => console.log('Scene:', sceneId)
const onHotspotSelect = (hotspot: unknown) => console.log('Selected:', hotspot)
</script>
```

## Ghi chú

- Tọa độ hotspot được tính trong hệ tọa độ của quả cầu bán kính 500; đặt điểm trên bề mặt quả cầu để hotspot xuất hiện đúng vị trí.
- Các `targetScene` phải trỏ tới cảnh đã khai báo.
- Component quản lý trạng thái điều khiển cục bộ (tự xoay, âm thanh, hotspot, FOV); các sự kiện giúp trang cha theo dõi thay đổi.
