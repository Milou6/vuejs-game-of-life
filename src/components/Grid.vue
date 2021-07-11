<template>
  <div class="grid">
    <!-- <div class="grid-row" v-for="x in lengthX" :key="'row' + x">
      <Cell v-for="y in lengthY" :key="x + ',' + y" :X="x" :Y="y"></Cell>
    </div> -->
    <button @click="setCellState(0, 1, 1)">Play</button>
    <button @click="startGrid">Step</button>
    <button @click="stopGrid">Stop</button>
    <div class="grid-row" v-for="(row, indexX) in cells" :key="'row' + indexX">
      <!-- <Cell v-for="cell in row" :isAlive="cell" :key="cell" @click="toggleCell($event)"></Cell> -->
      <Cell
        @toggle-cell="toggleCell"
        v-for="(cell, indexY) in row"
        :isAlive="cell"
        :key="'cell' + indexY"
        :X="indexX"
        :Y="indexY"
      ></Cell>
    </div>
  </div>
</template>

<script>
import Cell from './Cell.vue';

// this is the component 'options' object
export default {
  name: 'Grid',
  data() {
    return {
      // cells: new Array(10).fill([0, 0, 1, 0, 0, 0, 0, 0, 0, 0]),
      // cells: [
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 1, 1, 1, 1, 1, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      // ],
      // cells: [
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      //   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      // ],
      cells: [],
    };
  },
  created() {
    let cellsGrid = [];
    for (let x = 0; x < this.lengthX; x++) {
      let row = new Array(this.lengthY).fill(0);
      cellsGrid.push(row);
    }
    console.log(cellsGrid);
    this.cells = cellsGrid;
  },
  props: {
    msg: String,
    lengthX: Number,
    lengthY: Number,
  },
  computed: {},
  methods: {
    // setCellState(x, y, state) {
    //   let row = this.cells[x];
    //   console.log(row);
    //   this.cells[x].splice(y, 1, state);
    // },
    toggleCell(X, Y) {
      // console.log('Grid toggleCell: ', `X: ${X} Y: ${Y}`);
      const elm = this.cells[X][Y];
      this.cells[X].splice(Y, 1, elm === 0 ? 1 : 0);
    },
    startGrid() {
      this.interval = setInterval(() => {
        this.nextStepGrid();
      }, 50);
    },
    stopGrid() {
      clearInterval(this.interval);
    },
    nextStepGrid() {
      let newGrid = [];

      for (let i = 0; i < this.lengthX; i++) {
        let newRow = new Array(this.lengthX);
        for (let j = 0; j < this.lengthY; j++) {
          let neighb = this.getCellNeighbours(i, j);
          let liveNeigbours = 0;
          // let currentCell = this.cells[i][j];

          for (let n of neighb) {
            if (this.cells[n[0]][n[1]] == 1) {
              liveNeigbours += 1;
            }
          }
          // console.log(`liveNeigbours: ${liveNeigbours}`);

          // live cell w/ 2 or 3 neighbours survives
          if (this.cells[i][j] == 1) {
            if (liveNeigbours != 2 && liveNeigbours != 3) {
              // this.toggleCell(i, j);
              // console.log('alive to dead');
              newRow[j] = 0;
            } else {
              // console.log('alive, stas alive');
              newRow[j] = 1;
            }
          }
          // dead cell w/ 3 neighbours becomes alive
          else {
            if (liveNeigbours == 3) {
              // this.toggleCell(i, j);
              newRow[j] = 1;
              // console.log('dead to alive');
            } else {
              newRow[j] = 0;
            }
          }
        }
        // push newRow to newGrid
        newGrid.push(newRow);
      }

      // set grid as newGrid
      // console.log(newGrid);
      this.cells = newGrid;
    },
    getCellNeighbours(X, Y) {
      // console.log(X, Y);
      let neighb = [];

      for (let i = X - 1; i < X + 2; i++) {
        if (-1 < i && i < this.lengthX) {
          for (let j = Y - 1; j < Y + 2; j++) {
            if (-1 < j && j < this.lengthY) {
              if (JSON.stringify([i, j]) !== JSON.stringify([X, Y])) neighb.push([i, j]);
            }
          }
        }
      }
      return neighb;
    },
  },
  components: {
    Cell,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.grid-row {
  display: flex;
}
</style>
