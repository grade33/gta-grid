<template>
  <div id="app" class="app">
    <div class="grid">
      <div v-for="index in 16" :key="index" class="cell"></div>
      <div
        :draggable="false"
        class="handle"
        @mousedown.prevent="dragStart"
        :style="{ top: `${position.y}px`, left: `${position.x}px` }"
      ></div>
    </div>
    <div class="coordinates">
      Координаты: <span class="coordinates__value">({{ normalizedPosition.x }} : {{ normalizedPosition.y }})</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      position: { x: 140, y: 140 },
      grid: { width: 300, height: 300 },
      handleSize: 20,
      dragging: false,
    };
  },
  computed: {
    normalizedPosition() {
      // Нормализация координат
        return {
            x: +((this.position.x - this.grid.width / 2) / (this.grid.width / 2)).toFixed(2),
            y: -+((this.position.y - this.grid.height / 2) / (this.grid.height / 2)).toFixed(2)
        };
    },
  },
  methods: {
    dragStart(event) {
      this.dragging = true;
      this.start = {
        x: event.clientX - this.position.x,
        y: event.clientY - this.position.y,
      };
      window.addEventListener("mousemove", this.draggingHandler);
      window.addEventListener("mouseup", this.dragStop);
    },
    draggingHandler(event) {
      if (this.dragging) {
        let x = event.clientX - this.start.x;
        let y = event.clientY - this.start.y;
        x = Math.max(0, Math.min(x, this.grid.width)); // ограничение по ширине сетки
        y = Math.max(0, Math.min(y, this.grid.height)); // ограничение по высоте сетки
        this.position = { x, y };
      }
    },
    dragStop() {
      this.dragging = false;
      window.removeEventListener("mousemove", this.draggingHandler);
      window.removeEventListener("mouseup", this.dragStop);
    },
  },
};
</script>

<style lang="scss">
.app {
  display: flex;
  align-items: center;
  justify-content: center;
}
.grid {
  width: 323px;
  height: 323px;
  position: relative;
  z-index: 1;
  border-radius: 30px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(4, 1fr);
}

.cell {
  position: relative;
  border: 3px solid #4288b9;
  margin-top: -3px;
  margin-left: -3px;

  &:nth-child(1) {
    border-top-left-radius: 15px;
  }
  &:nth-child(4) {
    border-top-right-radius: 15px;
  }
  &:nth-child(13) {
    border-bottom-left-radius: 15px;
  }
  &:nth-child(16) {
    border-bottom-right-radius: 15px;
  }
}


.handle {
  width: 20px;
  height: 20px;
  background: #24588a;
  border-radius: 50%;
  position: absolute;
  z-index: 5;
  cursor: pointer;
}
.coordinates {
  margin-top: 20px;
    &__value {
      display: inline-block;
    }
}
</style>
