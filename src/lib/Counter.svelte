<script lang="ts">
  import { writable } from 'svelte/store';
  import { printOnConsole } from './tools/console';
  let count = 0;
  let result: string;
  let store = writable([]);
  $store.length;

  const increment = () => {
    count += 1;
    printOnConsole(`That is current count ${count}`);
    addTwoThings(5, String(count));
  };

  function addTwoThings(a: number, b: string): void {
    console.log('the result : ', a + b);
  }
</script>

<button on:click={increment}>
  Clicks: {count}
</button>

<style>
  button {
    font-family: inherit;
    font-size: inherit;
    padding: 1em 2em;
    color: #ff3e00;
    background-color: rgba(255, 62, 0, 0.1);
    border-radius: 2em;
    border: 2px solid rgba(255, 62, 0, 0);
    outline: none;
    width: 200px;
    font-variant-numeric: tabular-nums;
    cursor: pointer;
  }

  button:focus {
    border: 2px solid #ff3e00;
  }

  button:active {
    background-color: rgba(255, 62, 0, 0.2);
  }
</style>
