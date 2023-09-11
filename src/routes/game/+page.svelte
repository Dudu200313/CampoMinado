<script>
  import { row, col } from "../../store.js";
  let gameover = false;
  let grid = [];
  let carX = 0;
  let carY = 0;
  let bombs = $row * ($row / 4);
  let forceField = $row * ($row / 5);

  for (let i = 0; i < $row; i++) {
    grid[i] = new Array($col);
    for (let j = 0; j < $col; j++) {
      grid[i][j] = 0;
    }
  }

  for (let i = 0; i < bombs; i++) {
    let x = Math.floor(Math.random() * $row);
    let y = Math.floor(Math.random() * $col);
    if (
      grid[x][y] == 1 ||
      grid[x][y] == 2 ||
      (x == 0 && y == 0) ||
      (x == $row - 1 && y == $row - 1)
    ) {
      i--;
      continue;
    }
    grid[x][y] = 1;
  }

  for (let i = 0; i < forceField; i++) {
    let x = Math.floor(Math.random() * $row);
    let y = Math.floor(Math.random() * $col);
    if (
      grid[x][y] == 1 ||
      grid[x][y] == 2 ||
      (x == 0 && y == 0) ||
      (x == $row - 1 && y == $row - 1)
    ) {
      i--;
      continue;
    }
    grid[x][y] = 2;
  }

  function onKeyDown(e) {
    if (!gameover) {
      switch (e.keyCode) {
        case 37:
          carY -= 1;
          moverCarro();
          break;
        case 38:
          carX -= 1;
          moverCarro();
          break;
        case 39:
          carY += 1;
          moverCarro();
          break;
        case 40:
          carX += 1;
          moverCarro();
          break;
      }
    }
  }

  function moverCarro() {
    console.log(grid);
    console.log("CarX: " + carX + ", CarY: " + carY);
    if (grid[carX][carY] == 1) {
      gameover = true;
      console.log("Bateu numa bomba!");
    } else if (grid[carX][carY] == 2) {
      console.log("Achasse um campo de forÃ§a!");
    }
  }

  function getBoxClass(box, i, j) {
    if (box === 1) {
      return "bomb";
    } else if (box === 2) {
      return "force-field";
    } else if (carX === i && carY === j) {
      return "car";
    }
    return "";
  }
</script>

{#if gameover}
  <h1 style="font-size: 200px;">Gameover</h1>
{/if}

<svelte:window on:keydown|preventDefault={onKeyDown} />
<div class="game" style="--row: {$row}; --col: {$col};">
  {#each grid as $row, i}
    {#each $row as box, j}
      <div class={getBoxClass(box, i, j)} />
    {/each}
  {/each}
  <a href="/" class="start-button">Voltar a tela inicial</a>
</div>

<style>
  .game {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 400px;
    height: 400px;
    display: flex;
    flex-wrap: wrap;
  }

  .game > div {
    width: calc(100% / var(--col));
    height: calc(100% / var(--row));
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    border: 1px solid #111;
    box-sizing: border-box;
    cursor: pointer;
    position: relative;
  }
  .game > div.car {
    background-color: yellow; /* Cor do carro */
  }
  .game > div.force-field {
    background-color: aquamarine;
  }
  .game > div.bomb {
    background-color: red;
  }

  /* Estilos para o botão fictício */
  .start-button {
    display: inline-block;
    padding: 12px 24px;
    background-color: #007bff;
    color: #fff;
    text-align: center;
    transform: translate(63%, 10%);
    text-decoration: none;
    border-radius: 5px;
    transition: background-color 0.3s;
    cursor: pointer;
  }
</style>
