<script setup lang="ts">
import { ref } from "vue";
import X from "./components/X.vue";
import O from "./components/O.vue";

let grid = ref([
  [0, 0, 0],
  [0, 0, 0],
  [0, 0, 0],
]);

let turn = ref(1);
let xScore = ref(0);
let yScore = ref(0);

function move(x: number, y: number) {
  if (grid.value[x][y] !== 0) {
    return;
  }
  if (turn.value === 1) {
    grid.value[x][y] = 1;
  } else {
    grid.value[x][y] = 2;
  }
  winCheck();
  turn.value = turn.value == 1 ? (turn.value = 2) : (turn.value = 1);
}

function winCheck() {
  for (let [x, row] of grid.value.entries()) {
    for (let [y, cell] of row.entries()) {
      if (cell !== 0) {
        if (row[y + 1] == cell && row[y - 1] == cell) {
          // Horizontal
          win();
        } else if (x === 0) {
          // Top Row
          if (grid.value[x + 1][y] == cell && grid.value[x + 2][y] == cell) {
            // Vertical
            win();
          } else if (y === 0) {
            // First Cell
            if (
              grid.value[x + 1][y + 1] == cell &&
              grid.value[x + 2][y + 2] == cell
            ) {
              // Diagonal Left
              win();
            }
          } else if (y === 2) {
            // Last Cell
            if (
              grid.value[x + 1][y - 1] == cell &&
              grid.value[x + 2][y - 2] == cell
            ) {
              // Diagonal Right
              win();
            }
          }
        }
      }
    }
  }
}

function win() {
  setTimeout(() => {
    reset();
    if (turn.value == 1) {
      xScore.value++;
    } else {
      yScore.value++;
    }
  }, 1000);
}

function reset() {
  for (let x = 0; x < grid.value.length; x++) {
    for (let y = 0; y < grid.value.length; y++) {
      grid.value[x][y] = 0;
    }
  }
  turn.value = 1;
}
</script>

<template>
  <h1>Tic-Tac-Toe</h1>
  <div id="actionButtons">
    <!-- <button id="tutorialButton">Tutorial</button> -->
  </div>
  <div id="game">
    <div id="scoreCount">
      <div id="xScore" class="score">{{ xScore }}</div>
      <div id="oScore" class="score">{{ yScore }}</div>
    </div>
    <table id="grid">
      <tr v-for="x in 3">
        <td
          v-for="y in 3"
          :id="`${x - 1}.${y - 1}`"
          @click="move(parseInt(`${x - 1}`), parseInt(`${y - 1}`))"
        >
          <X v-if="grid[x - 1][y - 1] === 1" class="x" />
          <O v-if="grid[x - 1][y - 1] === 2" class="o" />
        </td>
      </tr>
    </table>
  </div>
</template>

<style scoped>
h1 {
  font-family: "Bebas Neue", sans-serif;
  font-weight: 400;
  font-style: normal;
  font-size: clamp(28px, 3rem, 80px);
  margin: 0px;
}

#actionButtons {
  margin-bottom: 50px;
}

button {
  border: 0;
  outline: none;
  border-radius: 8px;
  padding: 10px 20px;
  font-size: clamp(12px, 0.5rem, 24px);
  font-weight: 600;
  background-color: var(--accent);
  color: var(--text);

  &:hover {
    cursor: pointer;
  }
}

table,
tr,
td {
  border: 10px solid var(--accent);
  border-collapse: collapse;
}

td {
  --cell-dimensions: clamp(100px, 15vw, 300px);
  --icon-dimensions: clamp(50px, 7vw, 200px);
  width: var(--cell-dimensions);
  height: var(--cell-dimensions);
  text-align: center;

  &:hover {
    cursor: pointer;
  }

  .x {
    width: var(--icon-dimensions);
    height: var(--icon-dimensions);
    color: var(--x);
  }

  .o {
    width: var(--icon-dimensions);
    height: var(--icon-dimensions);
    color: var(--o);
  }
}

#game {
  display: flex;
  align-items: center;
  flex-direction: row;

  padding-right: 50px;

  #scoreCount {
    position: relative;
    right: 50px;

    .score {
      margin: 10vh 0px;
      font-size: 2rem;
    }

    #xScore {
      color: var(--x);
    }

    #oScore {
      color: var(--o);
    }
  }

  #grid {
    margin: auto;
  }
}
</style>
