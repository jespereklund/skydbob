<script>
  import Game from "./Game.svelte";

  let gameState = "first"
  let score
  let level = 2

  function gameOver(e) {
    score = e.detail.score
    gameState = "gameOver"
  }
</script>
<main>
  <div class="outer">
    {#if gameState === "first"}
      <div class="inner">
        <br><br>
        <p>Du skal skyde Bob så mange gange som muligt i løbet af et minut!</p>
        <br><br><br><br>
        <button on:click={e => {gameState = "selectDifficulty"}}>Start Game</button>
      </div>
    {:else if gameState === "selectDifficulty"}  
      <div class="inner">
        <p>Vælg sværhedsgrad:</p>
        <br><br>
        <button on:click={e => {level = 1; gameState="game"}}>Begynder</button>
        <br><br><br>
        <button on:click={e => {level = 2; gameState="game"}}>Øvet</button>
        <br><br><br>
        <button on:click={e => {level = 3; gameState="game"}}>Expert</button>
      </div>
    {:else if gameState === "game"}
      <Game on:gameOver={gameOver} {level} />
    {:else if gameState === "gameOver"}
      <div class="inner">
        <br><br>
        <p>Game Over, you got {score} points! </p>
        <br><br><br><br>
        <button on:click={e => {gameState = "selectDifficulty"}}>Play again</button>
      </div>
    {/if}
</div>
</main>
<style>
  button {
    width: 200px;
    height: 50px;
    border: 3px solid yellow;
    font-size: 20px;
    font-family: 'Courier New', Courier, monospace;
    font-weight: bolder;
    background-color: #339933;
    color: #ffff00;
  }

  p {
    color: #ffff00;
    font-weight: bolder;
    font-family: 'Courier New', Courier, monospace;
    font-size: 40px;
  }
  
  .outer {
    text-align:center;
    display: flex;
    justify-content: center;
    margin-inline: auto;
    background-color: #666666;
    width: 710px;
    height: 632px;
  }
</style>