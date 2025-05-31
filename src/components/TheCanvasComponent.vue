<template>
  <div class="canvas-container">
    <ImagesComponent />
  </div>

</template>

<script setup lang="ts">
import ImagesComponent from './ImagesComponent.vue';
</script>

<style lang="scss">
.canvas-container {
  width: 100vw;
  height: 100vh;
  background-color: red;
  background-position: 50% 50%;
  background-image: radial-gradient(rgba(128, 128, 128, 0.25) 1px, transparent 1px);
  background-size: 30px 30px;
  background-color: #101010;

  .my-canvas {
    position: absolute;

    img {
      pointer-events: none;
      user-select: none;
      max-width: 300px;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
      transition: all 0.15s cubic-bezier(.25, .8, .25, 1);
      transition-duration: 0.5s;
    }

    &:active img {
      box-shadow: 0 19px 38px rgba(0, 0, 0, 0.30), 0 15px 12px rgba(0, 0, 0, 0.22);
    }

    &:hover {
      cursor: grab;
    }

    &:active {
      cursor: grabbing;
    }
  }
}

#drop_zone {
  border: 5px solid blue;
  width: 200px;
  height: 100px;
}
</style>
