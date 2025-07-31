<script setup>
import { ref, onMounted } from 'vue';
import VueDragResizeRotate from '@gausszhou/vue3-drag-resize-rotate';

const zoom = ref(1);
const width = ref(300);
const height = ref(300);
const mainCanvas = ref(null);

const wheelHandler = ({ movement: [x, y], wheeling }) => {
  if (!wheeling) return;
  if (y > 0 && zoom.value > 1) {
    zoom.value -= 0.1;
  } else if (y < 0 && zoom.value < 5) {
    zoom.value += 0.1;
  }
};

onMounted(() => {
  const canvas = mainCanvas.value;
  if (canvas) {
    const { clientWidth, clientHeight } = canvas;
    const scale = Math.min(clientWidth, clientHeight) / 300;
    zoom.value = scale;
  }
});
</script>

<template>
  <div
    ref="mainCanvas"
    v-wheel="wheelHandler"
    class="relative flex h-[80vh] w-full items-center justify-center bg-gray-100 rounded-md overflow-hidden"
  >
    <VueDragResizeRotate
      :style="{ scale: zoom }"
      :h="height"
      :w="width"
      :scaleRatio="zoom"
      :draggable="true"
      :resizable="false"
      class-name="border border-dashed border-gray-400 bg-white"
    >
      <div class="text-center p-4">
        <p class="text-sm text-gray-600">Canvas Area</p>
        <p class="text-xs">Drag to move. Use mouse wheel to zoom.</p>
      </div>
    </VueDragResizeRotate>
  </div>
</template>
