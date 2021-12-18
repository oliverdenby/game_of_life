<template>
  <div id="app">
    <GameGrid ref="gameGrid"></GameGrid>
    <button @click="$refs.gameGrid.startGame()">Start</button>
    <button @click="$refs.gameGrid.stopGame()">Stop</button>
  </div>
</template>

<script>
import GameGrid from './components/GameGrid.vue'
export default {
  name: 'App',
  components: {
    GameGrid
  },
  data() {
    return {
      
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

div.cell-grid {
  background-color: azure;
  width: fit-content;
}

div.cell-row {
  font-size: 0;
}

div.cell {
  display: inline-block;
  border: black 1px solid;
  width: 50px;
  height: 50px;
}

div.cell.alive {
    background-color: grey;
}
div.cell.dead {
    background-color: white;
}

</style>
