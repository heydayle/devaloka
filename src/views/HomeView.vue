<script setup lang="ts">
import Cube3D from '@/components/Cube3D.vue';

</script>

<template>
  <main style="text-align: center;">
    <Cube3D style="position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); z-index: -1;" />
    <p style="font-size: 200px; margin-bottom: -50px;">Devaloka</p>
    <p class="content">
      Infinite realm of creation, where
      language and the
      cosmos resonate.
    </p>
    <div style="margin-top: 32px;">
      <p class="content">
        <a href="https://thinh.io.vn" target="_blank"
          style="color: goldenrod; border-bottom: 1px solid goldenrod;padding-bottom: 0; padding: 0 12px 0px;">
          @Thinh Le
        </a>
      </p>
      <p class="content">2025</p>
    </div>
  </main>
</template>
<style>
.content {
  font-size: 24px;
  font-family: 'Shavina Elegance';
  margin-bottom: 0;
}
</style>
