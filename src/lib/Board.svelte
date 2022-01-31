<script lang="ts">
  export let board :number[][];
</script>

<div
  class="board"
  style:grid-template-columns={`repeat(${board.length}, 32px)`}
>
  {#each board as array, row}
    {#each array as item, col}
      <div>{item}</div>
    {/each}
  {/each}
</div>

<style lang="scss">
  .board {
    background: #f8f8f8;
    display: grid;
    grid-auto-rows: 32px;
    justify-content: center;
    align-items: center;
    max-width: 100%;
    height: calc(100vh - 300px);
    overflow: scroll;
  }
</style>
