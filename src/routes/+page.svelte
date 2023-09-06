<script>
  import { row, col } from '../store.js'
  import { goto } from '$app/navigation';
  import "../app.css";
  
  function startGame(){
    goto('game')
  }
</script>

<div class="minesweeper-container">
  <label for="row-input" class="input-label">Rows</label>
  <input id="row-input" class="input-field" bind:value={$row} />

  <label for="col-input" class="input-label">Columns</label>
  <input id="col-input" class="input-field" bind:value={$col} />

  <button class="start-button" on:click={() => startGame($row, $col)}>Start Game</button>
</div>
<style>
.minesweeper-container {
  text-align: center;
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.input-container {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.input-label {
  font-size: 35px;
  margin-right: 10px; /* Reduzi a margem para alinhar melhor as labels */
}

.input-field {
  width: 50px;
  height: 30px;
  font-size: 16px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px; /* Adicionei uma margem à direita para separar os campos */
}

.start-button {
  background-color: #4CAF50;
  color: white;
  padding: 10px 20px;
  font-size: 15px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  margin-top: 15px; /* Adicionei uma margem superior para separar o botão */
}

.start-button:hover {
  background-color: #45a049;
}
</style>