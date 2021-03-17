<template>
  <div class="container">
    <div class="score"> Score: {{ score }} </div>
    <div class="board">
      <template v-for="(row, i) in grid">
        <div
          v-for="(ele, j) in row"
          :key="i + '-' + j"
          :class="'cell ' + (ele !== 0 ? 'exists' : '')"
        >
          <div :class="'inner ' + `tile-${ele}`">
            {{ ele !== 0 ? ele : "" }}
          </div>
        </div>
      </template>
    </div>
  </div>
</template>

<script lang='ts'>
import { Component, Vue } from 'vue-property-decorator'
import { Direction } from '../types/directions'

@Component
export default class Board extends Vue {
  private grid = [
    [0, 0, 0, 0],
    [0, 0, 0, 2],
    [0, 0, 0, 2],
    [0, 2, 2, 4]
  ]

  get score () {
    return this.grid.flat().reduce((prev, curr) => prev + curr)
  }

  private move (direction: KeyboardEvent) {
    const oldGrid = [...this.grid]
    switch (direction.key) {
      case 'ArrowDown':
        console.log('ArrowDown', Direction.Down)
        this.grid = this.moveDown(this.grid)
        break
      case 'ArrowUp':
        console.log('ArrowUp', Direction.Up)
        this.grid = this.moveUp(this.grid)
        break
      case 'ArrowLeft':
        console.log('ArrowLeft', Direction.Left)
        this.grid = this.moveLeft(this.grid)
        break
      case 'ArrowRight':
        console.log('ArrowRight', Direction.Right)
        this.grid = this.moveRight(this.grid)
        break
      default:
        break
    }
    if (this.gridMoved(oldGrid, this.grid)) {
      this.grid = this.insertTile(this.grid)
    }
  }

  private gridMoved (oldGrid: number[][], newGrid: number[][]) {
    return !oldGrid.flat().every((ele, idx) => ele === newGrid.flat()[idx])
  }

  private insertTile (grid: number[][], val = 2) {
    const emptySpots: number[][] = []
    grid.forEach((row, x) => {
      row.forEach((ele, y) => {
        if (ele === 0) {
          emptySpots.push([x, y])
        }
      })
    })
    const randomIndex = Math.floor(emptySpots.length * Math.random())
    const [randX, randY] = emptySpots[randomIndex]
    const newGrid = [...grid]
    newGrid[randX][randY] = val
    return newGrid
  }

  private transpose = (m: number [][]) => m[0].map((x, i) => m.map(x => x[i]))

  private moveUp (grid: number [][]) {
    return this.transpose(this.moveLeft(this.transpose(grid)))
  }

  private moveDown (grid: number[][]) {
    return this.transpose(this.moveRight(this.transpose(grid)))
  }

  private moveLeft (grid: number[][]) {
    const newGrid = grid.map(row => {
      const nonZeroRow = row.filter(ele => ele !== 0)
      const newRow = nonZeroRow.concat(Array(row.length - nonZeroRow.length).fill(0))
      return newRow
    })
    for (let i = 0; i < newGrid.length; i++) {
      for (let j = 1; j < newGrid[i].length; j++) {
        const ele = newGrid[i][j]
        const leftNeighbour = newGrid[i][j - 1]
        if (leftNeighbour === 0) {
          [newGrid[i][j - 1], newGrid[i][j]] = [ele, 0]
          continue
        }
        if (leftNeighbour === ele) {
          [newGrid[i][j - 1], newGrid[i][j]] = [ele + leftNeighbour, 0]
          continue
        }
      }
    }
    return newGrid
  }

  private moveRight (grid: number [][]) {
    const newGrid = grid.map(row => {
      const nonZeroRow = row.filter(ele => ele !== 0)
      const newRow = Array(row.length - nonZeroRow.length).fill(0).concat(nonZeroRow)
      return newRow
    })
    for (let i = 0; i < newGrid.length; i++) {
      for (let j = newGrid[i].length - 1; j >= 0; j--) {
        const ele = newGrid[i][j]
        const rightNeighbour = newGrid[i][j + 1]
        if (rightNeighbour === 0) {
          [newGrid[i][j + 1], newGrid[i][j]] = [ele, 0]
          continue
        }
        if (rightNeighbour === ele) {
          [newGrid[i][j + 1], newGrid[i][j]] = [ele + rightNeighbour, 0]
          continue
        }
      }
    }
    return newGrid
  }

  mounted () {
    document.addEventListener('keydown', this.move)
  }

  destroyed () {
    document.removeEventListener('keydown', this.move)
  }
}
</script>

<style>
.score {
  font-size: 5vh;
}
.container {
  width: 600px;
}
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
  font-size: 5vh;
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
