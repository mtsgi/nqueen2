<script lang="ts">
  import logo from './assets/nqueen_logo.png'
  import engine from './assets/queen_engine.png'
  import type { putPosition } from "./lib/types";

  import Board from './lib/Board.svelte';

  let n = 4;
  let board: number[][] = [];

  let queenCount = 0;
  let fillCount = 0;

  const onStart = () => {
    board = new Array();

    for (let i = 0; i < n; i++) {
      board.push(new Array());
      for (let k = 0; k < n; k++) {
        board[i].push(0);
      }
    }
  }

  const onPut = (event: CustomEvent<putPosition>) => {
    const row = event.detail.row;
    const col = event.detail.col;

    if (board[row][col] === 0) {
      board[row][col] = 2;
      queenCount++;
      fillCount++;
      const sr = Math.max(row + 1, n - row);
      const sc = Math.max(col + 1, n - col);
      const loop = Math.max(sr, sc);
      console.log({ sr, sc, loop });

      for (let i = 0; i < loop; i++) {
        [
          [row - i, col - i],
          [row + i, col + i],
          [row + i, col - i],
          [row - i, col + i],
          [row, col + i],
          [row, col - i],
          [row + i, col],
          [row - i, col]
        ].forEach(([nrow, ncol]) => {
          if (nrow < n && nrow >= 0 && ncol < n && ncol >= 0) {
            if (board[nrow][ncol] === 0) {
              board[nrow][ncol] = 1;
              fillCount++;
            }
          }
        });
      }
    }
  }
</script>

<main>
  <header>
    <img src={logo} alt="N Queen 2" />

    <input type=number bind:value={n} min=4 />
    <button on:click={onStart}>Start</button>
  </header>

  <div class="wrapper">
    <Board {board} on:put={onPut} />
  </div>

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
    height: 160px;
    display: flex;
    gap: 10px;
    flex-direction: column;
    align-items: center;
    > img {
      margin-top: 10px;
      height: 60px;
    }
    > input {
      font-family: inherit;
      font-size: inherit;
      border: none;
      border-radius: 8px;
      background: #f0f0f0;
      padding: 5px;
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

  .wrapper {
    margin: 0 20px;
    max-width: 100%;
    height: calc(100vh - 230px);
    overflow: scroll;
    scrollbar-width: thin;
  }
  
  footer {
    background: #f0f0f0;
    height: 70px;
    > img {
      margin: 10px;
      height: 50px;
    }
  }
</style>
