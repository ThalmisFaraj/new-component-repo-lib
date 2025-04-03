<script>
  import { createEventDispatcher } from "svelte";
  export let options = [];
  export let selected = null;
  export let placeholder = "Select an option";
  let isOpen = false;

  const dispatch = createEventDispatcher();

  function selectOption(option) {
    selected = option;
    isOpen = false;
    dispatch("change", option);
  }
</script>

<div class="relative inline-block w-52" on:click={() => (isOpen = !isOpen)}>
  <div
    class="p-3 border border-gray-300 bg-white rounded-md shadow-sm cursor-pointer"
  >
    {selected || placeholder}
  </div>
  {#if isOpen}
    <ul
      class="absolute w-full bg-white border border-gray-300 rounded-md shadow-md mt-1 z-10"
    >
      {#each options as option}
        <li
          class="p-3 hover:bg-gray-100 cursor-pointer"
          on:click={() => selectOption(option)}
        >
          {option}
        </li>
      {/each}
    </ul>
  {/if}
</div>
