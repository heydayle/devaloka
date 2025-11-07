<script setup lang="ts">
import Cube3D from '@/components/Cube3D.vue';

</script>

<template>
  <main style="text-align: center;">
    <Cube3D style="position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); z-index: -1;" />
    <p style="font-size: 200px; margin-bottom: -50px;">Heyday</p>
    <p style="font-size: 13px">Nobody's ever believed in me... I simply believed in myself</p>
    <div style="margin-top: 32px;text-align: left;">
      <p class="content" style="margin-top:16px">
        >_ Hello world_
      </p>
      <p class="content list">
        <a href="https://thinh.io.vn" target="_blank">
          >_ mine
        </a>
        <a href="https://memories.thinh.io.vn" target="_blank">
          >_ gallery
        </a>
      </p>
        <p class="since">since 2025</p>
    </div>
  </main>
</template>
<style>
.content {
  font-size: 24px;
  font-family: monospace;
  margin-bottom: 0;
}
.list {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
a {
  color: rgba(64, 64, 253, 0.89);
}
a:hover {
  background: transparent;
  color: goldenrod !important;
}
.since {
  margin-top: 16px;
  font-family: monospace;
}
</style>
