<script lang="ts">
  import { fade, slide } from 'svelte/transition';

  import logo from './assets/nqueen_logo.png'
  import engine from './assets/queen_engine.png'
  import type { putPosition } from "./lib/types";

  import Board from './lib/Board.svelte';

  let n = 4;
  let board: number[][] = [];
  $: size = board.length;

  let queenCount = 0;
  let fillCount = 0;

  // Game Status
  let isCleared = false;
  let isFailed = false;

  let tweetUrl = '';

  const onStart = () => {
    queenCount = 0;
    fillCount = 0;
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
      const sr = Math.max(row + 1, size - row);
      const sc = Math.max(col + 1, size - col);
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
          if (nrow < size && nrow >= 0 && ncol < size && ncol >= 0) {
            if (board[nrow][ncol] === 0) {
              board[nrow][ncol] = 1;
              fillCount++;
            }
          }
        });
      }

      if (queenCount === n) {
        isCleared = true;
        const squares = board.map(
          row => row.map(item => item === 2 ? "🟦" : "⬜").join("")
        ).join("%0a");
        tweetUrl = `https://twitter.com/intent/tweet?text=${size} Queen%0a%0a${squares}`;
      }

      else if (fillCount === size * size) {
        isFailed = true;
      }
    }
  }

  const closeWindow = () => {
    isCleared = false;
    isFailed = false;
  }
</script>

<main>
  <header>
    <img src={logo} alt="N Queen 2" />

    <div class="form">
      <input type=number bind:value={n} min=4 />
      <button on:click={onStart}>Start</button>
    </div>
  </header>

  <div class="wrapper">
    <Board {board} on:put={onPut} />
  </div>

  <footer>
    <div class="stats">
      Queen {queenCount}/{size}
      |
      Filled {fillCount}/{size*size}
    </div>
    <img src={engine} alt="Queen Engine 3" />
  </footer>

  {#if isCleared}
    <div class="window" transition:fade|local>
      <div class="window__card" transition:slide|local>
        <h2>{n} Queen Cleared!</h2>
        <a
          href={tweetUrl}
          target="_blank"
          class="window__card--tweet"
        >Tweet</a>
        <button
          class="window__card--close"
          on:click={closeWindow}
        >close</button>
      </div>
    </div>
  {/if}

  {#if isFailed}
    <div class="window" transition:fade|local>
      <div class="window__card" transition:slide|local>
        <h2>Failed...</h2>
        <button
          class="window__card--close"
          on:click={closeWindow}
        >close</button>
      </div>
    </div>
  {/if}
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
    height: 140px;
    display: flex;
    gap: 10px;
    flex-direction: column;
    align-items: center;

    > img {
      margin: 10px;
      height: 60px;
    }

    .form {
      display: flex;

      > input {
        font-family: inherit;
        font-size: inherit;
        border: none;
        border-radius: 8px 0 0 8px;
        background: #f0f0f0;
        padding: 8px;
        accent-color: #5fd1fb;
      }

      > button {
        cursor: pointer;
        font-family: inherit;
        font-size: inherit;
        padding: 8px 20px;
        color: #f0f0f0;
        background: linear-gradient(135deg, rgba(95,209,251,1) 0%, rgba(69,103,231,1) 100%);
        border: none;
        border-radius: 0 8px 8px 0;
      }
    }
  }

  .wrapper {
    padding: 20px;
    max-width: 100%;
    box-sizing: border-box;
    height: calc(100vh - 240px);
    overflow: scroll;
    scrollbar-width: thin;
  }
  
  footer {
    background: #f0f0f0;
    height: 100px;
    .stats {
      font-size: 14px;
      color: #8497B0;
      padding: 5px;
    }
    > img {
      margin: 10px;
      height: 50px;
    }
  }

  .window {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(255, 255, 255, 0.5);
    display: grid;
    align-items: center;
    justify-content: center;

    &__card {
      width: 260px;
      background: #ffffff;
      border-radius: 6px;
      box-shadow: 0 4px 16px 0 rgba(0, 0, 0, 0.2);
      display: flex;
      flex-direction: column;
      padding: 20px;

      > h2 {
        margin: 10px 0 20px 0;
      }

      &--tweet {
        cursor: pointer;
        color: #ffffff;
        font-family: inherit;
        font-size: inherit;
        padding: 5px 20px;
        margin-bottom: 10px;
        border-radius: 5px;
        border: 2px solid #54a6f3;
        background: #54a6f3;
        text-decoration: none;
      }

      &--close {
        cursor: pointer;
        font-family: inherit;
        font-size: inherit;
        padding: 5px 20px;
        border-radius: 5px;
        border: 2px solid #f0f0f0;
        background: #ffffff;
      }
    }
  }
</style>
