<template>
  <div class="cell-grid">
    <div class="cell-row" v-for="(cellRow, i) in cells" :key="i">
      <Cell
        v-for="(cell, j) in cells[i]"
        :key="j"
        :id="i + ',' + j"
        :isAlive_="cells[i][j]"
        @cellClicked="handleCellClicked"
      >
      </Cell>
    </div>
  </div>
</template>

<script>
import Cell from "./Cell.vue";
export default {
  name: "GameGrid",
  components: {
    Cell,
  },
  data() {
    return {
      cells: [
        [true, false, true],
        [true, true, true],
        [false, true, false],
      ],
      gameTimer: null
    };
  },
  methods: {
    handleCellClicked(id) {
      const coords = id.split(",");
      const y = parseInt(coords[0]);
      const x = parseInt(coords[1]);
      this.cells[y][x] = !this.cells[y][x];
    },
    startGame() {
      this.checkBorders();
      this.gameTimer = setInterval(this.gameCycle, 250)
    },
    stopGame() {
      clearInterval(this.gameTimer)
    },
    gameCycle() {
      const cellsToSwapState = [];
      for (let i = 0; i < this.cells.length; i++) {
        for (let j = 0; j < this.cells[i].length; j++) {
          const liveNeighbours = this.checkNeighbours(i, j)
          if (this.cells[i][j]) {
            if (liveNeighbours < 2 || liveNeighbours > 3) cellsToSwapState.push([i, j])
          }
          else {
            if (liveNeighbours == 3) cellsToSwapState.push([i, j])
          }
        }        
      }
      cellsToSwapState.forEach(coords => {
        const [y, x] = coords
        this.cells[y][x] = !this.cells[y][x]
      })  
      this.checkBorders()
    },
    checkTopBorder() {
      return this.cells[0].some(cell => cell == true)
    },
    checkRightBorder() {
      let found = false
      for (const cellRow of this.cells) {
         if (cellRow[this.cells[0].length-1]) {
           found = true
           break
         }
      }
      return found
    },
    checkBottomBorder() {
      return this.cells[this.cells.length-1].some(cell => cell == true)
    },
    checkLeftBorder() {
      let found = false
      for (const cellRow of this.cells) {
        if (cellRow[0]) {
          found = true
          break
        }
      }
      return found
    },
    checkBorders() {
      if(this.checkTopBorder()) this.addTopBorder()
      if(this.checkRightBorder()) this.addRightBorder()
      if(this.checkBottomBorder()) this.addBottomBorder()
      if(this.checkLeftBorder()) this.addLeftBorder()
    },
    addTopBorder() {
      const borderTop = new Array(this.cells[0].length);
      borderTop.fill(false)
      this.cells.unshift(borderTop);
    },
    addRightBorder() {
      this.cells.forEach(cellRow => cellRow.push(false)) 
    },
    addBottomBorder() {
      const borderBottom = new Array(this.cells[0].length);
      borderBottom.fill(false)
      this.cells.push(borderBottom);
    },
    addLeftBorder() {
      this.cells.forEach(cellRow => cellRow.unshift(false))
    },

    checkNeighbours(y, x) {
      let count = 0;
      try {
        if (this.cells[y-1][x-1]) count++
      } catch (error) {
        //do nothing
      }
      try {
        if (this.cells[y-1][x]) count++
      } catch (error) {
        //do nothing
      }
      try {
        if (this.cells[y-1][x+1]) count++
      } catch (error) {
        //do nothing
      }
      try {
        if (this.cells[y][x-1]) count++
      } catch (error) {
        //do nothing
      }
      try {
        if (this.cells[y][x+1]) count++
      } catch (error) {
        //do nothing
      }
      try {
        if (this.cells[y+1][x-1]) count++
      } catch (error) {
        //do nothing
      }
      try {
        if (this.cells[y+1][x]) count++
      } catch (error) {
        //do nothing
      }
      try {
        if (this.cells[y+1][x+1]) count++
      } catch (error) {
        //do nothing
      }
      return count
    },

  },
};
</script>