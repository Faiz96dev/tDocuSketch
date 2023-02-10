<script setup>
import { onMounted } from "@vue/runtime-core";
import { computed } from "@vue/reactivity";
// Get canvas and context
const canvas = computed(() => document.getElementById("roomCanvas"));

const ctx = computed(() => {
  return canvas.value.getContext("2d");
});

// Room shapes
const simpleRoom = [
[0, 0],
  [100, 0],
  [100, 50],
  [50, 50],
  [50, 50]
]

const triangleRoom = [  [0, 0],
  [100, 0],
  [50, 50]
];

const tShapeRoom = [  [0, 0],
  [100, 0],
  [100, 50],
  [50, 50],
  [50, 100]
];
// Room dimensions
const dimensions = [
  [0, 0, 100, 0],
  [0, 0, 0, 50],
];

// Randomly select room shape and dimensions
let room = simpleRoom;
let length = dimensions[0];
let width = dimensions[1];

// Function to draw room shape
const drawRoom = () => {
  ctx.value.clearRect(0, 0, canvas.width, canvas.height);
  ctx.value.beginPath();
  ctx.value.moveTo(room[0][0], room[0][1]);
  for (let i = 1; i < room.length; i++) {
    ctx.value.lineTo(room[i][0], room[i][1]);
  }
  ctx.value.closePath();
  ctx.value.stroke();
};


// Function to draw room dimensions
const drawDimensions = () => {
  ctx.value.beginPath();
  ctx.value.moveTo(length[0], length[1]);
  ctx.value.lineTo(length[2], length[3]);
  ctx.value.stroke();
  ctx.value.beginPath();
  ctx.value.moveTo(width[0], width[1]);
  ctx.value.lineTo(width[2], width[3]);
  ctx.value.stroke();
};

onMounted(() => {
  room = simpleRoom
  drawRoom();
  document.getElementById("changeDimensions").addEventListener("click", () => {
    const roomShapes = [simpleRoom, triangleRoom, tShapeRoom];
    room = roomShapes[Math.floor(Math.random() * roomShapes.length)];
    length = dimensions[Math.floor(Math.random() * dimensions.length)];
    width = dimensions[Math.floor(Math.random() * dimensions.length)];
    drawRoom();
    drawDimensions();
  });
});
// Change dimensions button click event
</script>

<template>
  <div class="wrapper">
    <canvas id="roomCanvas" width="500" height="500"></canvas>
    <button id="changeDimensions">Change Dimensions</button>
  </div>
</template>

<style scoped>
.canvas-room {
  display: flex;
  justify-content: center;
}
.wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
