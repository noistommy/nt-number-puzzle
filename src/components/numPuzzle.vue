<script setup>
import { ref, reactive, onMounted } from "vue";

const gridSize = 4;
let numbers = Array.from({ length: gridSize * gridSize - 1 }, (_, i) => i + 1);
const complate = ref(true);
const matrix = ref([]);
const empty = ref({});
const actived = ref({});
onMounted(() => {
  matrix.value = setGrid();
});

const setGrid = () => {
  empty.value = { x: gridSize - 1, y: gridSize - 1 };
  actived.value = { x: 0, y: 0 };
  return numbers.map((item, i) => {
    const cell = {
      x: i % gridSize,
      y: Math.floor(i / gridSize),
      id: item,
    };
    return cell;
  });
};

function suffleGrid() {
  numbers = numbers.sort(() => Math.random() - 0.5);
  matrix.value = setGrid();
  complate.value = false;
}

function moveCell(target) {
  if (complate.value) {
    console.log("Press start button!");
    return;
  }

  let move = matrix.value[target];

  const { x, y } = empty.value;
  if (
    (move.y === y && Math.abs(move.x - x) === 1) ||
    (move.x === x && Math.abs(move.y - y) === 1)
  ) {
    empty.value = { x: move.x, y: move.y };
    move.x = x;
    move.y = y;
  }
  actived.value.x = move.x;
  actived.value.y = move.y;
  if (empty.value.x === gridSize - 1 && empty.value.y === gridSize - 1) {
    complate.value = matrix.value.every(
      (c, i) => c.x + c.y * gridSize === c.id - 1
    );
  }
}
function moveActive(dir) {
  console.log(dir);
  if (dir === "up") actived.value.y--;
  if (dir === "down") actived.value.y++;
  if (dir === "left") actived.value.x--;
  if (dir === "right") actived.value.x++;
}
</script>

<template>
  <div class="title">
    <h1>Number Puzzle</h1>
  </div>
  <div
    class="puzzle"
    tabindex="-1"
    :style="{ '--grid-size': gridSize }"
    @keypress.up="moveActive('up')"
    @keypress.down="moveActive('down')"
  >
    <div class="header">
      <button class="reset-btn" @click="suffleGrid">
        {{ complate ? "START" : "RESET" }}
      </button>
    </div>
    <div class="cell-wrapper">
      <template v-for="(cell, i) in matrix" :key="i">
        <span
          class="cell"
          :class="{ active: actived.x === cell.x && actived.y === cell.y }"
          :style="{ '--x': cell.x, '--y': cell.y }"
          :data-name="cell.id"
          @click="moveCell(i)"
        ></span>
      </template>
    </div>
  </div>
</template>

<style lang="scss">
$issue_red: #D75757;

.header {
  display:flex;
  justify-content: flex-end;
  margin-bottom: 10px;
  .reset-btn {
    //position: absolute;
    //top: 0;
    //left: calc(100% + 5px);
    background: linear-gradient(180deg, lighten($issue_red, 10), darken($issue_red, 30));
    padding: 0.56em 1em;
    border-radius: 0.4em;
    color: #fff;
    font-weight: 700;
    //display: flex;
    //justify-content: center;
    //align-items: center;
    font-size: 1em;
    outline: none;
    border: 0;

  }
}
    

.puzzle {
  --grid-size: 4;
  --cell-size: 4rem;
  --cell-gap: 0.4rem;
  //position: fixed;
  //left: 0;
  //top: 0;
  //background-color: #aaa;
  border-radius: 0.5em;
  //padding: 8px 5px 5px;
  margin: 0 auto;
  width: calc(var(--cell-size) * var(--grid-size) + var(--cell-gap) * (var(--grid-size) + 1));
  height: calc(var(--cell-size) * var(--grid-size) + var(--cell-gap) * (var(--grid-size) + 1));
  max-width: 600px;
  max-height: 600px;
  .cell-wrapper {
    position: relative;
    border-radius: 0.4em;
    background-color: #666;
    width: 100%;
    height: 100%;
    .cell {
      background-color: #ccc;
      //box-shadow: -2px -2px 5px 0 #454545 inset;
      border-radius: 0.1em;
      position: absolute;
      width: var(--cell-size);
      height: var(--cell-size);
      top: calc(var(--y) * (var(--cell-size) + var(--cell-gap)) + var(--cell-gap));
      left: calc(var(--x) * (var(--cell-size) + var(--cell-gap)) + var(--cell-gap));
      transition: 250ms linear;
      &:before {
        content: '';
        position: absolute;
        inset: 2px;
        background-color: #fff;
        opacity: 0.3;
      }
      &:after {
        content: attr(data-name);
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        font-weight: 900;
        font-size: 1.2em;
        color: #333;
      }
      &.active {
        background-color: #ffffff;
        box-shadow: 0 0 30px 0 #fff;
      }
    }
  }
}


</style>