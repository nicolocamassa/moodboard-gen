<template>
  <div v-for="image in images" :key="image.id" @mousedown="(event: MouseEvent) => startDragging(event, image.id)"
    @mouseup="endDragging(image.id)" class="my-canvas" :id="image.id.toString()"
    :style="{ top: image.top + 'px', left: image.left + 'px', zIndex: image.zindex }">
    <img :src="image.src" :alt="image.alt" />
  </div>
</template>
<script setup lang="ts">

/* TODO: QUESTIONE LIVELLI AL CLICK */
/* ! Risolvere il problema di quando continua a trascinare immagine quando cursore su un'altra immagine  */

import imageUrl from '@/assets/image.png';
import { onMounted, reactive } from 'vue';

const images = reactive([
  {
    src: "https://i.pinimg.com/736x/f9/33/31/f933313ebb4110c48afcd10d1010201f.jpg",
    alt: "Image",
    id: 3,
    top: 560,
    left: 730,
    isDragging: false,
    zindex: 0
  },
  {
    src: "https://i.pinimg.com/736x/ea/17/92/ea179213a7cf5597b779863a320a27eb.jpg",
    alt: "Image",
    id: 2,
    top: 400,
    left: 620,
    isDragging: false,
    zindex: 0
  },
  {
    src: imageUrl,
    alt: "Image",
    id: 1,
    top: 500,
    left: 500,
    isDragging: false,
    zindex: 0
  }
])

// Posizione iniziale immagine cliccata
let startLeft = 0;
let startTop = 0;

// Posizione iniziale click mouse
let startMouseX = 0;
let startMouseY = 0;

const sidebarWidth = 320;

// Assegnazione livelli al caricamento
onMounted(() => {
  images.forEach((element, index) => {
    element.zindex = index + 1;
  })
})

/* *CLICK INIZIALE */
function startDragging(event: MouseEvent, id: number) {

  images.forEach((element /* ,index */) => {
    if (id == element.id) {
      const image = document.getElementById(element.id.toString());

      if (!image) return;

      // Coordinate del mouse all’inizio del drag
      startMouseX = event.clientX;
      startMouseY = event.clientY;

      // Estrazione della posizione corrente
      startLeft = image.offsetLeft;
      startTop = image.offsetTop;

      // Flag per seguire il dragging
      element.isDragging = true;

      // Inizia il tracciamento delle coordinate del mouse
      if (event.button == 0) {
        window.addEventListener("mousemove", (event) => trackCoordinates(event, id))
      }
    }
  });
}

// *MOVIMENTO MOUSE E TRACCIAMENTO COORDINATE
function trackCoordinates(event: MouseEvent, id: number) {

  images.forEach(element => {
    if (id == element.id) {
      if (element.isDragging) {

        const image = document.getElementById(element.id.toString());
        if (!image) return;

        // Coordinate mouse e canvas
        const { clientX: mouseX, clientY: mouseY } = event;
        const { x: canvasX, y: canvasY, width, height } = image.getBoundingClientRect();

        // Calcolo della posizione del click per lo spostamento
        const deltaX = event.clientX - startMouseX;
        const deltaY = event.clientY - startMouseY;

        if (image.parentElement) {
          const limitedArea = image.parentElement.getBoundingClientRect();

          // Calcolo limite spostamento
          const
            x = Math.min(
              Math.max(limitedArea.left + sidebarWidth, startLeft + deltaX),
              limitedArea.right - width
            ),

            y = Math.min(
              Math.max(limitedArea.top, startTop + deltaY),
              limitedArea.bottom - height
            );

          // Spostamento dell'immagine
          if (image) {
            element.top = y;
            element.left = x;
          }
        }

        console.log("🖱️ MOVED coordinates:", { mouseX, mouseY, canvasX, canvasY });
      }
    }
  });
}

// *STOP DEL DRAGGING
function endDragging(id: number) {
  images.forEach(element => {
    if (id == element.id) {
      element.isDragging = false
    }
  });
}
</script>
<style lang="scss"></style>
