<script lang="ts">
  import { createEventDispatcher } from "svelte";

  import type { putPosition } from "./types";
  export let board :number[][];

  const dispatch = createEventDispatcher<{
    put: putPosition,
    hover: putPosition,
    leave: void
  }>();

  const clickItem = (row: number, col: number) => {
    dispatch("put", { row, col });
  }

  const hoverItem = (row: number, col: number) => {
    if (board[row][col] === 0) dispatch("hover", { row, col });
  }

  const leaveItem = () => dispatch("leave");
</script>

<div
  class="board"
  style:grid-template-columns={`repeat(${board.length}, 32px)`}
>
  {#each board as array, row}
    {#each array as item, col}
      <div
        class="board__item"
        class:filled={item > 0}
        on:mouseenter={() => hoverItem(row, col)}
        on:mouseleave={leaveItem}
        on:click={() => clickItem(row, col)}
      >
        {#if item === 2}
          ♛
        {/if}
      </div>
    {/each}
  {/each}
</div>

<style lang="scss">
  .board {
    display: grid;
    grid-auto-rows: 32px;
    gap: 8px;
    justify-content: safe center;
    align-items: stretch;
    box-sizing: border-box;

    &__item {
      cursor: pointer;
      border-radius: 3px;
      border: 2px solid #e0e0e0;
      line-height: 32px;
      transition: .2s all ease;
      transform: rotateX(180deg);
      color: #ffffff;

      &:hover {
        border: 2px solid #a0a0a0;
      }

      &.filled {
        cursor: default;
        border: none;
        background: #54a6f3;
        transform: rotateX(0deg);

        &:hover {
          border: none
        }
      }
    }
  }
</style>
