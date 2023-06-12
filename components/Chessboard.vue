<script setup lang="ts">
import { ref, computed, VNodeRef } from 'vue';
import { TheChessboard } from 'vue3-chessboard';
import 'vue3-chessboard/style.css';
import type { BoardApi, BoardConfig } from 'vue3-chessboard';

const boardAPI = ref<BoardApi>();
const boardConfig: BoardConfig = {
  coordinates: false,
  autoCastle: false,
};


function handleCheckmate(isMated: string) {
  if (isMated === 'w') {
    alert('Black wins!');
  } else {
    alert('White wins!');
  }
}

const width = computed(() => innerWidth)
const height = computed(() => innerHeight)

const screenAspect = computed(() => width.value / height.value)

const scale = computed(() => {
  
  if (screenAspect.value < (16/9))
    return width.value / 980
  return height.value * (16/9) / 980
})
</script>

<template>
  <section>
    <div class="flex flex-row gap-3">
      <button @click="boardAPI?.toggleOrientation()" class="p-1 border rounded-sm">
        Toggle orientation
      </button>
      <button @click="boardAPI?.resetBoard()" class="p-1 border rounded-sm">Reset</button>
      <button @click="boardAPI?.undoLastMove()" class="p-1 border rounded-sm">Undo</button>
      <button @click="boardAPI?.toggleMoves()" class="p-1 border rounded-sm">Threats</button>
    </div>
    <TheChessboard
      :board-config="boardConfig"
      @board-created="(api) => (boardAPI = api)"
      @checkmate="handleCheckmate"
      id="chessboard"
      :style="{
        'transform': `scale(${1 / scale})`
      }"
      class="p-0 m-0 text-center w-96"
    />

  </section>
</template>

<style scoped>
  #chessboard piece {
    background-color: blueviolet;
  }
</style>