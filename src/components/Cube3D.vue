<template>
  <div class="container">
    <div class="fps" style="color: white;z-index:10">{{ fps }}</div>
    <div ref="threeContainer" class="three-container"></div>
  </div>
</template>

<script lang="ts">
import { ref, onMounted, onBeforeUnmount, defineComponent } from 'vue';
import Stats from 'stats.js'
// eslint-disable-next-line @typescript-eslint/ban-ts-comment
// @ts-ignore
import * as THREE from 'three'

export default defineComponent({
  setup() {

    const fps = ref(0)  
    const stats = new Stats()
    stats.showPanel(0)
    document.body.appendChild(stats.dom)

    const threeContainer = ref<HTMLElement | null>(null);
    let scene: THREE.Scene;
    let camera: THREE.PerspectiveCamera;
    let renderer: THREE.WebGLRenderer;
    let triangle: THREE.Mesh;
    let animationFrameId: number;

    const initThreeJs = (): void => {
      if (!threeContainer.value) return;

      // Create scene
      scene = new THREE.Scene();

      // Create camera
      camera = new THREE.PerspectiveCamera(
        60,
        threeContainer.value.clientWidth / threeContainer.value.clientHeight,
        0.1,
        1000
      );
      camera.position.z = 5;

      // Create renderer
      renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
      renderer.setSize(threeContainer.value.clientWidth, threeContainer.value.clientHeight);
      renderer.setClearColor(0x121212, 1);
      threeContainer.value.appendChild(renderer.domElement);

      // Add ambient light
      const ambientLight = new THREE.AmbientLight(0x040404, 0.2);
      scene.add(ambientLight);

      // Main directional light from above
      const topLight = new THREE.DirectionalLight(0xffffff, 1);
      topLight.position.set(0, 5, 2);
      topLight.castShadow = true;
      scene.add(topLight);

      // Fill light for better edge visibility
      const fillLight = new THREE.DirectionalLight(0x9370DB, 0.3);
      fillLight.position.set(0, -2, 1);
      scene.add(fillLight);

      // Create 3D triangle
      createTriangle();

      // Start animation loop
      animate();
    };

    const createTriangle = (): void => {
      // Create a tetrahedron (basic 3D triangle)
      const geometry = new THREE.TetrahedronGeometry(1.5, 0);

      // Use a reflective material
      const material = new THREE.MeshStandardMaterial({
        color: '#ffd324',      // Yellow color
        metalness: 0.3,        // Metallic level
        roughness: 0.4,        // Roughness
        flatShading: true,     // Flat shading to see faces clearly
        emissive: 0x0,         // No emission
        side: THREE.DoubleSide // Show both sides
      });

      // Create mesh from geometry and material
      triangle = new THREE.Mesh(geometry, material);

      // Rotate for better initial view
      triangle.rotation.x = Math.PI / 6;
      // triangle.rotation.y = Math.PI / 4;

      // Add mesh to scene
      scene.add(triangle);
    };

    // Animation function for continuous gentle rotation
    const animate = (): void => {
      stats.begin()
      animationFrameId = requestAnimationFrame(animate);

      if (triangle) {
        // Continuous gentle rotation
        // triangle.rotation.y += 0.01;
        triangle.rotation.x += 0.002;

        // Add subtle sin movement for natural feel
        // triangle.position.y = 0.1 * Math.sin(Date.now() * 0.001);
      }
      stats.end()
      renderer.render(scene, camera);
    };

    // Handle window resize
    const handleResize = (): void => {
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

      // Clean up resources
      if (triangle) {
        triangle.geometry.dispose();
        triangle.material.dispose();
      }

      if (renderer) {
        renderer.dispose();
      }

      // Remove canvas
      if (threeContainer.value && renderer) {
        threeContainer.value.removeChild(renderer.domElement);
      }
    });

    return {
      threeContainer,
      fps
    };
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
