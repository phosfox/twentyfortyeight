<template>
  <div class="board">
      <template v-for="(row, i) in grid" >
          <div v-for="(ele, j) in row" :key="i + '-' + j" :class="'cell ' + (ele !== 0 ? 'exists' : '')">
            <div :class="'inner ' + `tile-${ele}`">
                {{ele !== 0 ? ele : ''}}
            </div>
          </div>
      </template>
  </div>
</template>

<script lang='ts'>
import { Component, Vue } from 'vue-property-decorator'

@Component
export default class Board extends Vue {
  get grid (): number[][] {
    return [[0, 0, 0, 0], [0, 0, 0, 2], [0, 0, 0, 2], [0, 0, 2, 4]]
  }
}
</script>

<style>
.board {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    background: gray;
}
.cell {
    /* background-color: lightgrey; */
    width: 150px;
    height: 150px;
    /* border: 1px solid black; */
    padding: 5px;
    box-sizing: border-box;
    flex-basis: 25%;
}

.cell > .inner {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #d3d3d3;
    border-radius: 10px;
    font-size: 5vw;
}
.cell.exists > .inner.tile-2 {
    background: #f0edea;
}
.cell.exists > .inner.tile-4 {
    background: #eee1c9;
}
.cell.exists > .inner.tile-8 {
    background: #f3b27a;
}
.cell.exists > .inner.tile-16 {
    background: #f69664;
}
.cell.exists > .inner.tile-32 {
    background: #f77c5f;
}
.cell.exists > .inner.tile-64 {
    background: #f75f3b;
}
.cell.exists > .inner.tile-128 {
    background: #edd073;
}
.cell.exists > .inner.tile-256 {
    background: #edcc62;
}
.cell.exists > .inner.tile-512 {
    background: #edc950;
}
.cell.exists > .inner.tile-1024 {
    background: #edc53f;
}
.cell.exists > .inner.tile-2048 {
    background: #edc22e;
}
</style>
