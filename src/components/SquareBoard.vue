<script setup lang="ts">
interface CellProps {
  player?: string | null;
}
const props = defineProps<{
  elem: CellProps;
}>();
const emit = defineEmits(["clickeditem"]);

const toggleSymbol = () => {
  emit("clickeditem", props.elem);
};
</script>

<template>
  <div class="containerBoard" @click="toggleSymbol">
    <svg
      v-if="elem.player === 'X'"
      class="symbol x-symbol"
      viewBox="0 0 100 100"
    >
      <line x1="10" y1="10" x2="90" y2="90" />
      <line x1="90" y1="10" x2="10" y2="90" />
    </svg>

    <svg
      v-else-if="elem.player === 'O'"
      class="symbol o-symbol"
      viewBox="0 0 100 100"
    >
      <circle cx="50" cy="50" r="40" />
    </svg>
  </div>
</template>

<style lang="css" scoped>
.containerBoard {
  height: 50px;
  border: 1px solid black;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.symbol {
  width: 100%;
  height: 100%;
}

.x-symbol line {
  stroke: red;
  stroke-width: 8;
  stroke-linecap: round;
}

.o-symbol circle {
  stroke: blue;
  stroke-width: 8;
  fill: none;
}
</style>
