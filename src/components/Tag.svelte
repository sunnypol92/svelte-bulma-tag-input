<div class="control">
  <span class="tags has-addons">
    <span class="tag is-primary is-marginless">{tag}</span>
    {#if !disabled}
      <span class="tag is-delete is-marginless" on:click={remove}></span>
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