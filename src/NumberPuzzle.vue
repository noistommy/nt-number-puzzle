<script setup>
import { ref, reactive, onMounted } from "vue";

const props = defineProps({
  gridSize: Number,
  cellSize: {
    type: Number,
    default: 3
  },
  labelText: String,
  counting: {
    type: Boolean,
    default: false
  }
})

const emits = defineEmits(['complate']);

const {gridSize, cellSize, labelText, counting} = props;

let numbers = Array.from({ length: gridSize * gridSize - 1 }, (_, i) => { return { id: i, text: i + 1}});
let lastWord = { x: gridSize - 1, y: gridSize - 1, item: gridSize * gridSize};

const complate = ref(true);
const matrix = ref([]);
const empty = ref({});
const actived = ref({});
const showOrder = ref(false);
const counter = ref(0);

onMounted(() => {
  if(labelText) {
    lastWord = { x: gridSize - 1, y: gridSize - 1, item: labelText[gridSize * gridSize - 1]}
    numbers = labelText.split('').slice(0, gridSize * gridSize - 1).map( (item, i) => { return {id: i, text: item }})
  }
  matrix.value = setGrid();
});

const setGrid = () => {
  empty.value = { x: gridSize - 1, y: gridSize - 1 };
  actived.value = { x: 0, y: 0 , id: 0};
  return numbers.map((item, i) => {
    const cell = {
      x: i % gridSize,
      y: Math.floor(i / gridSize),
      id: item.id,
      label: item.text
    };
    return cell;
  });
};

function retryGrid() {
  numbers = numbers.sort(() => Math.random() - 0.5);
  matrix.value = setGrid();

  complate.value = false;
  counter.value = 0;
}

function moveCell(target) {
  if (complate.value) {
    console.log("Press start button!");
    return;
  }
  console.log(actived.value)
  let move = matrix.value[target];

  const { x, y } = empty.value;
  if (
    (move.y === y && Math.abs(move.x - x) === 1) ||
    (move.x === x && Math.abs(move.y - y) === 1)
  ) {
    counter.value++;
    empty.value = { x: move.x, y: move.y };
    move.x = x;
    move.y = y;
  }
  actived.value.x = move.x;
  actived.value.y = move.y;
  actived.value.id = findCell(move)
  if (empty.value.x === gridSize - 1 && empty.value.y === gridSize - 1) {
    setTimeout(() => {
      complate.value = matrix.value.every(
        (c, i) => c.x + c.y * gridSize === c.id
      );
      if(complate.value) emits('complate', counter.value)
    }, 200)
    
  }
}

function findCell({x, y}) {
  return matrix.value.findIndex(item => item.x === x && item.y === y)
}

const moveUp = () => {
  if(actived.value.y === 0) return;
  actived.value.y--
  actived.value.id = findCell(actived.value)
}
const moveDown = () => {
  if(actived.value.y === gridSize - 1) return;
  actived.value.y++
  actived.value.id = findCell(actived.value)}
const moveLeft = () => {
  if(actived.value.x === 0) return;
  actived.value.x--
  actived.value.id = findCell(actived.value)}
const moveRight = () => {
  if(actived.value.x === gridSize - 1) return;
  actived.value.x++
  actived.value.id = findCell(actived.value)}
</script>

<template>
  <!-- <div class="title">
    <h1>Number Puzzle </h1>
  </div> -->
  <div
    class="puzzle"
    tabindex="0"
    :style="{ '--grid-size': gridSize,  '--cell-size': `${cellSize}rem` }"
    @keydown.up="moveUp"
    @keydown.down="moveDown"
    @keydown.left="moveLeft"
    @keydown.right="moveRight"
    @keydown.space="moveCell(actived.id)"
    @keydown.ctrl="showOrder = !showOrder"
  >
    <div class="header">
      <div class="point">
        <div class="counter" v-if="counting">{{ counter }}</div>
      </div>
      <button class="retry-btn" @click="retryGrid">
        {{ complate ? "START" : "RETRY" }}
      </button>
    </div>
    <div class="cell-wrapper" :class="{complate: complate}">
      <template v-for="(cell, i) in matrix" :key="i">
        <div
          class="cell"
          :class="{ active: actived.x === cell.x && actived.y === cell.y }"
          :style="{ '--x': cell.x, '--y': cell.y }"
          :data-name="cell.label"
          @click="moveCell(i)"
        ><span class="index" v-show="showOrder">{{ cell.id }}</span></div>
        <div class="cell last" :style="{ '--x': lastWord.x, '--y': lastWord.y }" :data-name="lastWord.item"></div>
      </template>
    </div>
  </div>
</template>

<style lang="scss">
$issue_red: #D75757;

.header {
  display:flex;
  justify-content: space-between;
  margin-bottom: 10px;
  .counter {
    font-size: 2em;
    font-weight: bold;
    line-height: 1em;
  }
  .retry-btn {
    //position: absolute;
    //top: 0;
    //left: calc(100% + 5px);
    // background: linear-gradient(180deg, lighten($issue_red, 10), darken($issue_red, 30));
    background-color: $issue_red;
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
      span.index {
        position: absolute;
        top: 0;
        right: 0;
        padding: 0 5px;
        color: #333;
        background-color: #fff;
        font-size: 0.8em;
      }
    }
    .last {
      display: none;
    }
    &.complate .last {
      display: block;
    }
  }
}


</style>