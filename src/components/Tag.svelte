<div class="control">
  <span class="tag is-primary">
    {tag}
    {#if !disabled}
      <button class="delete is-small" on:click={remove}></button>
    {/if}
  </span>
</div>

<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  export let index = null;
  export let tag = null;
  export let disabled = false;

  function remove() {
    dispatch('remove', {index});
  }
</script>