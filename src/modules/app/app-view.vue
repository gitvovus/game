<script setup lang="ts">
import { type App } from './app';

const { model } = defineProps<{ model: App }>();

function keydown(e: KeyboardEvent) {
  if (e.code === 'Escape') {
    model.figureSelector.select('queen');
  }
}
const letters = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H'];
const numbers = [8, 7, 6, 5, 4, 3, 2, 1];
</script>

<template>
  <div class="flex col gap-4">
    <teleport to="body">
      <ui-dialog :model="model.dialog" class="figure-selector-dialog" @keydown="keydown">
        <div class="dlg-panel">
          <div class="flex ai-center px-2">
            <h3>dialog</h3>
            <div class="spacer" />
          </div>
          <div class="dlg-content">
            <figure-selector :model="model.figureSelector" />
          </div>
        </div>
      </ui-dialog>
    </teleport>
    <!-- <h1>Workin</h1> -->
    <div class="grid">
      <div class="board-header flex row ai-center gap-4">
        <ui-button class="btn" @click="model.reset()">reset</ui-button>
        <div>
          turn: {{ model.chess.turn }}
        </div>
      </div>
      <figure-selector class="selector-item" :model="model.figureSelector" />
      <div class="board-border"></div>
      <div class="board-side top row">
        <span v-for="i in letters" :key="i">{{ i }}</span>
      </div>
      <div class="board-side left col">
        <span v-for="i in numbers" :key="i">{{ i }}</span>
      </div>
      <div class="board-item">
        <board-view :model="model.board" />
      </div>
      <div class="board-side right col">
        <span v-for="i in numbers" :key="i">{{ i }}</span>
      </div>
      <div class="board-side bottom row">
        <span v-for="i in letters" :key="i">{{ i }}</span>
      </div>
      <!-- <div class="grid-item" v-for="i in 9" :key="i"></div> -->
    </div>
  </div>
</template>

<style lang="scss">
#app {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.grid {
  --base: min(10vw, 7vh);
  width: calc(var(--base) * 9);
  height: calc(var(--base) * 13);
  position: relative;
  display: grid;
  grid-template-columns: 1fr 4fr 8fr 4fr 1fr;
  grid-template-rows: 2fr 2fr 1fr 16fr 1fr 4fr;
}

.grid-item {
  border: 1px solid darkred;
}

.board-header {
  grid-column-start: 1;
  grid-column-end: -1;
  grid-row-start: 1;
}

.board-border {
  border: 2px solid rgb(var(--border));
  grid-column-start: 1;
  grid-column-end: -1;
  grid-row-start: 3;
  grid-row-end: 6;
}

.board-side {
  // background-color: black;
  &.left {
    grid-row-start: 4;
    grid-column-start: 1;
  }
  &.right {
    grid-row-start: 4;
    grid-column-start: 5;
  }
  &.top {
    grid-column-start: 1;
    grid-column-end: -1;
    grid-row-start: 3;
  }
  &.bottom {
    grid-column-start: 1;
    grid-column-end: -1;
    grid-row-start: 5;
  }
  &.top, &.bottom {
    padding-inline: calc(var(--base) / 2);
  }
}

.board-item {
  position: relative;
  grid-column-start: 2;
  grid-column-end: -2;
  grid-row-start: 4;
  grid-row-end: 5;
}

.selector-item {
  position: relative;
  grid-column-start: 3;
  grid-row-start: 2;
}

$width: 400px;
$height: 150px;
dialog.figure-selector-dialog {
  left: calc((100% - $width) / 2);
  top: 2em;
  width: $width;
  height: $height;
}

.dlg-panel {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  pointer-events: none;
  border-radius: var(--radius-large);
  border: 1px solid rgb(var(--border));
  background-color: rgb(var(--surface));
  box-shadow: var(--shadow-large);
  overflow: hidden;
}

.dlg-content {
  position: relative;
  flex: 1 1 auto;
  pointer-events: auto;
  overflow: hidden;
}

.slow-enter-from,
.slow-leave-to {
  opacity: 0;
}
.slow-enter-active,
.slow-leave-active {
  transition: opacity var(--slow);
}

.fast-enter-from,
.fast-leave-to {
  opacity: 0;
}
.fast-enter-active,
.fast-leave-active {
  transition: opacity var(--fast);
}

.delayed-enter-from,
.delayed-leave-to {
  opacity: 0;
}
.delayed-enter-active {
  transition: opacity var(--fast) calc(var(--slow) - var(--fast));
}
.delayed-leave-active {
  transition: opacity var(--fast);
}
</style>
