<template>
  <div @mousedown="startDragging" @mouseup="endDragging" class="my-canvas" id="workspace"></div>
</template>

<script setup lang="ts">

let isDragging: boolean = false;
let startMouseX = 0;
let startMouseY = 0;
let startLeft = 0;
const startTop = 0;


/* *CLICK INIZIALE */
function startDragging(event: MouseEvent) {

  const workspace = document.getElementById("workspace")
  if (!workspace) return;

  // Coordinate del mouse all’inizio del drag
  startMouseX = event.clientX;
  startMouseY = event.clientY;

  // Estrazione della posizione corrente
  startLeft = workspace.offsetLeft;

  // Flag per seguire il dragging
  isDragging = true;

  removeEventListener("mouseup", trackCoordinates)

  // Inizia il tracciamento delle coordinate del mouse
  if (event.button == 0) {
    workspace.addEventListener("mousemove", trackCoordinates)
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
  const { x: canvasX, y: canvasY } = workspace.getBoundingClientRect();

  // Calcolo della posizione del click per lo spostamento
  const deltaX = event.clientX - startMouseX;
  const deltaY = event.clientY - startMouseY;

  // Spostamento
  if (workspace) {
    workspace.style.left = `${startLeft + deltaX}px`;
    workspace.style.top = `${startTop + deltaY}px`;
  }

  console.log("🖱️ MOVED coordinates:", { mouseX, mouseY, canvasX, canvasY });
}

// *STOP DEL DRAGGING
function endDragging() {
  isDragging = false;
}
</script>

<style lang="scss" scoped>
body {
  overflow: hidden;
}

div {
  width: 200vw;
  height: 200vh;
  background-position: 50% 50%;
  background-image: radial-gradient(rgba(128, 128, 128, 0.25) 1px, transparent 1px);
  background-size: 30px 30px;
  background-color: #101010;
  border: 2px solid transparent;
  position: absolute;

  &:active {
    cursor: grab;
  }
}
</style>
