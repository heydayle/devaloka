<template>
  <div class="container">
    <div ref="threeContainer" class="three-container"></div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';
import * as THREE from 'three';

const threeContainer = ref(null);
let scene, camera, renderer, triangle;
let animationFrameId;

const initThreeJs = () => {
  // Tạo scene
  scene = new THREE.Scene();

  // Tạo camera
  camera = new THREE.PerspectiveCamera(
    60,
    threeContainer.value.clientWidth / threeContainer.value.clientHeight,
    0.1,
    1000
  );
  camera.position.z = 5;

  // Tạo renderer
  renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
  renderer.setSize(threeContainer.value.clientWidth, threeContainer.value.clientHeight);
  renderer.setClearColor(0x121212, 1);
  threeContainer.value.appendChild(renderer.domElement);

  // Thêm ánh sáng từ trên xuống
  const ambientLight = new THREE.AmbientLight(0x040404, 0.2);
  scene.add(ambientLight);

  // Ánh sáng chính từ trên xuống
  const topLight = new THREE.DirectionalLight(0xffffff, 1);
  topLight.position.set(0, 5, 2);
  topLight.castShadow = true;
  scene.add(topLight);

  // Ánh sáng phụ để tạo đủ độ sáng cho các cạnh
  const fillLight = new THREE.DirectionalLight(0x9370DB, 0.3);
  fillLight.position.set(0, -2, 1);
  scene.add(fillLight);

  // Tạo hình tam giác 3D
  createTriangle();

  // Bắt đầu animation loop
  animate();
};

const createTriangle = () => {
  // Tạo hình tứ diện (tetrahedron) - hình tam giác 3D cơ bản
  const geometry = new THREE.TetrahedronGeometry(1.5, 0);

  // Sử dụng material phản chiếu
  const material = new THREE.MeshStandardMaterial({
    color: '#ffd324',          // Màu xanh
    metalness: 0.3,           // Độ kim loại
    roughness: 0.4,           // Độ nhám
    flatShading: true,        // Phẳng để thấy rõ các mặt
    emissive: 0x0,            // Không phát sáng
    side: THREE.DoubleSide    // Hiển thị cả hai mặt
  });

  // Tạo mesh từ geometry và material
  triangle = new THREE.Mesh(geometry, material);

  // Xoay một chút để nhìn đẹp hơn khi bắt đầu
  triangle.rotation.x = Math.PI / 6;
  // triangle.rotation.y = Math.PI / 4;

  // Thêm mesh vào scene
  scene.add(triangle);
};

// Hàm animate cho hiệu ứng xoay liên tục nhẹ nhàng
const animate = () => {
  animationFrameId = requestAnimationFrame(animate);

  if (triangle) {
    // Xoay nhẹ nhàng liên tục
    // triangle.rotation.y += 0.01;
    triangle.rotation.x += 0.003;

    // Thêm chút chuyển động sin nhẹ để tạo cảm giác tự nhiên
    // triangle.position.y = 0.1 * Math.sin(Date.now() * 0.001);
  }

  renderer.render(scene, camera);
};

// Hàm xử lý khi kích thước cửa sổ thay đổi
const handleResize = () => {
  if (threeContainer.value && camera && renderer) {
    const width = threeContainer.value.clientWidth;
    const height = threeContainer.value.clientHeight;

    camera.aspect = width / height;
    camera.updateProjectionMatrix();
    renderer.setSize(width, height);
  }
};

onMounted(() => {
  initThreeJs();
  window.addEventListener('resize', handleResize);
});

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize);

  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId);
  }

  // Dọn dẹp resources
  if (triangle) {
    triangle.geometry.dispose();
    triangle.material.dispose();
  }

  if (renderer) {
    renderer.dispose();
  }

  // Xóa canvas
  if (threeContainer.value && renderer) {
    threeContainer.value.removeChild(renderer.domElement);
  }
});
</script>

<style scoped>
.container {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.three-container {
  width: 100%;
  height: 100vh;
  overflow: hidden;
  border-radius: 8px;
}
</style>
