<script>
  import { row, col } from '../store.js'
  import { goto } from '$app/navigation';
  
  function startGame(){
    goto('game')
  }
</script>

<label for="">Row</label>
<input bind:value={$row} />
<label for="">Column</label>
<input bind:value={$col} />
<button on:click={() => startGame($row, $col)}>Start Game</button>