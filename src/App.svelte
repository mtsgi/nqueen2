<script lang="ts">
  import logo from './assets/nqueen_logo.png'
  import engine from './assets/queen_engine.png'

  import Board from './lib/Board.svelte';

  let n = 4;
  let board: number[][] = [];

  const onStart = () => {
    board = Array(n).fill(Array(n).fill(0));
  }
</script>

<main>
  <header>
    <img src={logo} alt="N Queen 2" />

    <input type=number bind:value={n} min=4 />
    <button on:click={onStart}>Start</button>
  </header>

  <Board {board} />

  <footer>
    <img src={engine} alt="Queen Engine 3" />
  </footer>
</main>

<style lang="scss">
  :global(body) {
    margin: 0;
    overflow: hidden;
  }

  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    text-align: center;
  }
  
  header {
    height: 200px;
    display: flex;
    gap: 10px;
    flex-direction: column;
    align-items: center;
    > img {
      margin: 10px;
      height: 80px;
    }
    > input {
      font-family: inherit;
      font-size: inherit;
      border: none;
      border-radius: 8px;
      background: #f0f0f0;
      padding: 10px;
      accent-color: #5fd1fb;
    }
    > button {
      font-family: inherit;
      font-size: inherit;
      padding: 5px 20px;
      color: #f0f0f0;
      background: linear-gradient(90deg, rgba(95,209,251,1) 0%, rgba(69,103,231,1) 100%);
      border: none;
      border-radius: 100px;
    }
  }
  
  footer {
    background: #f0f0f0;
    height: 100px;
    > img {
      margin: 20px;
      height: 60px;
    }
  }
</style>
