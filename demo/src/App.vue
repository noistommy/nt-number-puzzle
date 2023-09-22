<script setup>
import HelloWorld from './components/HelloWorld.vue'
import NumberPuzzle from '../../src/NumberPuzzle.vue'
// import { NumberPuzzle } from '../../dist/nt-number-puzzle.es'
// import '../../dist/number-puzzle.css';

// const labels = ""
const labels = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"

const setComplate = (value) => {
  console.log(value)
}
</script>

<template>
  <HelloWorld msg="Vite + Vue" />
  <NumberPuzzle :grid-size="4" :cell-size="4" :label-text="labels" @complate="setComplate"/>
</template>

<style  lang="scss" scoped>

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
