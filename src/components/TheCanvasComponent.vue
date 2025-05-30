<template>
  <div class="canvas-container">
    <div @mousedown="startDragging" @mouseup="endDragging" class="my-canvas" id="workspace">
      <img src="/src/assets/image.png" alt="">
    </div>
  </div>

</template>

<script setup lang="ts">
let isDragging: boolean = false;
let startMouseX = 0;
let startMouseY = 0;
let startLeft = 0;
let startTop = 0;

/* *CLICK INIZIALE */
function startDragging(event: MouseEvent) {

  const workspace = document.getElementById("workspace")
  if (!workspace) return;

  // Coordinate del mouse all’inizio del drag
  startMouseX = event.clientX;
  startMouseY = event.clientY;

  // Estrazione della posizione corrente
  startLeft = workspace.offsetLeft;
  startTop = workspace.offsetTop;

  // Flag per seguire il dragging
  isDragging = true;

  removeEventListener("mouseup", trackCoordinates)

  // Inizia il tracciamento delle coordinate del mouse
  if (event.button == 0) {
    window.addEventListener("mousemove", trackCoordinates)
  }
}

// *MOVIMENTO MOUSE E TRACCIAMENTO COORDINATE
function trackCoordinates(event: MouseEvent) {
  if (!isDragging) return

  // Ottengo le coordinate dell'angolo del canvas
  const workspace = document.getElementById("workspace");
  if (!workspace) return;

  // Coordinate mouse e canvas
  const { clientX: mouseX, clientY: mouseY } = event;
  const { x: canvasX, y: canvasY, width, height } = workspace.getBoundingClientRect();

  // Calcolo della posizione del click per lo spostamento
  const deltaX = event.clientX - startMouseX;
  const deltaY = event.clientY - startMouseY;

  if (workspace.parentElement) {
    const limitedArea = workspace.parentElement.getBoundingClientRect();

    // Calcolo limite spostamento
    const
      x = Math.min(
        Math.max(limitedArea.left, startLeft + deltaX),
        limitedArea.right - width
      ),

      y = Math.min(
        Math.max(limitedArea.top, startTop + deltaY),
        limitedArea.bottom - height
      );

    console.log("Questo è min: ", limitedArea.right - width,)
    console.log("questa è x: ", startLeft + deltaX)
    // Spostamento dell'immagine
    if (workspace) {
      workspace.style.left = `${x}px`;
      workspace.style.top = `${y}px`;
    }
  }

  console.log("🖱️ MOVED coordinates:", { mouseX, mouseY, canvasX, canvasY });
}

// *STOP DEL DRAGGING
function endDragging() {
  isDragging = false;
}
</script>
<style lang="scss" scoped>
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
    }

    &:hover {
      cursor: grab;
    }

    &:active {
      cursor: grabbing;
    }
  }
}
</style>
