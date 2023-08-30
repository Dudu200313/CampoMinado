<script>
  import { row, col } from '../../store.js'
  let gameover = false
  let grid = []
  let bombs = $row * ($row/4)

  for(let i = 0; i < $row; i++){
    grid[i] = new Array($col)
    for(let j = 0; j < $col; j++){
      grid[i][j] = {
        bomb: false,
        revealed: false,
        distance: 0
      }
    }
  }

  for(let i = 0; i < bombs; i++){
    let x = Math.floor(Math.random()*$row)
    let y = Math.floor(Math.random()*$col)
    if(grid[x][y].bomb){
      i--
      continue
    }
    grid[x][y].bomb = true
  }
  
  function countDistance(i, j){
    let count = 0
    for(let x = -1; x <= 1; x++){
      for(let y = -1; y <= 1; y++){
        try{
          if(grid[x+i][y+j].bomb){
            count++
          }
        } catch(msg){}
      }
    }
    grid[i][j].distance = count
  }
  for(let i = 0; i < $row; i++){
    for(let j = 0; j < $col; j++){
      countDistance(i, j)
    }
  }
  function revealBox(i, j){
    grid[i][j].revealed = true
    if(grid[i][j].bomb){
      gameover = true
      for(let i = 0; i < $row; i++){
        for(let j = 0; j < $col; j++){
          if(grid[i][j].bomb){
            grid[i][j].revealed = true
          }
        }
      }
    }
    if(grid[i][j].distance == 0){
      for(let x = -1; x <= 1; x++){
        for(let y = -1; y <= 1; y++){
          try{
            if(!grid[x+i][y+j].bomb && !grid[x+i][y+j].revealed){
              revealBox(x+i, y+j)
            }
          }catch(msg){}
        }
      }
    }
  }
</script>

<style>
  .game{
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 400px;
    height: 400px;
    display: flex;
    flex-wrap: wrap;
  }

  .game > div{
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
  .game > div.bomb:before{
    content: "";
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 20px;
    height: 20px;
    background: #111;
    border-radius: 50%;
  }
  .game > div.revealed{
    background: #ddd;
  }
</style>

{#if gameover}
  <h1>Gameover</h1>
{/if}

<div class="game" style="--row: {$row}; --col: {$col};">
  {#each grid as $row, i}
    {#each $row as box, j}
      {#if box.revealed}
        <div class:bomb={box.bomb} class:revealed={box.revealed}>
          {#if box.distance > 0}
            {box.distance}
          {/if}
        </div>
      {:else}
        <div on:click={()=>{revealBox(i, j)}}></div>
      {/if}
    {/each}
  {/each}
</div>