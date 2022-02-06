<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import type { putPosition } from "./types";
  export let board :number[][];

  const dispatch = createEventDispatcher<{ put: putPosition }>();

  const clickItem = (row: number, col: number) => {
    console.log("clicked", row, col);
    dispatch("put", { row, col });
  }
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
    justify-content: left;
    align-items: stretch;
    box-sizing: border-box;

    &__item {
      cursor: pointer;
      border-radius: 3px;
      border: 2px solid #f0f0f0;
      line-height: 32px;

      &:hover {
        border: 2px solid #d0d0d0;
      }

      &.filled {
        border: none;
        background: #ffa2a2;

        &:hover {
          border: none
        }
      }
    }
  }
</style>
