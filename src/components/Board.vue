<template>
  <div>
    <table>
      <tr v-for="row in grid" :key="row">
          <td v-for="cell in row" :key="cell">
              {{cell}}
          </td>
      </tr>
    </table>
  </div>
</template>

<script lang='ts'>
import { Component, Vue } from 'vue-property-decorator'

@Component
export default class Board extends Vue {
  get grid (): number[] {
    return Array(4).fill(Array(4).fill(2))
  }
}
</script>

<style>
</style>
