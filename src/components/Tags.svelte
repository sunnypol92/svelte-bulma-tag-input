<div class="input is-{inputSize} is-{hasError ? 'danger' : ''} {disabled ? 'is-disabled' : ''} tag-selector">
  <div class="field is-grouped is-grouped-multiline">
    {#if tags.length > 0}
      {#each tags as tag, i}
        <div class="control">
          <span class="tag is-{tagColor} is-{tagSize}">{tag}
            {#if !disabled}
              <button class="delete is-{tagRemoveButtonSize}" on:click={() => removeByClick(i)}></button>
            {/if}
          </span>
        </div>
      {/each}
    {/if}

    {#if !_disabled}
      <div class="control is-expanded">
        <input
          class="input"
          type="text"
          placeholder={(tags.length == 0) ? placeholder : ''}
          disabled={_disabled}
          bind:value={tag}
          bind:this={tagInput}
          on:keydown={createByKey}
          on:keydown={removeByKey}
          on:paste={createByPaste}
          on:drop={createByDrop}
        >
      </div>
    {/if}

    {#if disabled && tags.length == 0 && placeholder}
      <div class="control is-expanded">
        <input class="input" type="text" {placeholder} disabled={_disabled}>
      </div>
    {/if}
  </div>
</div>

<p class="help has-text-grey is-marginless">
  <small class="has-text-success">+ {addKeyNames.length ? addKeyNames.join(' / ') : 'No keys defined'}</small> |

  {#if removeKeyNames.length}
    <small class="has-text-danger">- {removeKeyNames.join(' / ')}</small> |
  {/if}

  {#if maxTags}
    <small><span class="has-text-success">{_tags_left}</span>/<span class="has-text-danger">{maxTags}</span></small>
  {/if}
</p>

<script>
  import {createEventDispatcher} from 'svelte';
  import KeyMap from '../helpers/keyMap.js';

  const dispatch = createEventDispatcher();

  let tagInput;
  let tag = '';
  let addKeyNames = [];
  let removeKeyNames = [];
  let tagRemoveButtonSize = 'small';
  let sizeMap = ['small', 'normal', 'medium', 'large'];

  export let tags = [];
  export let addKeys = [13];
  export let removeKeys = [8];
  export let maxTags = 5;
  export let splitWith = ' ';
  export let allowDuplicates = false;
  export let allowPaste = false;
  export let allowDrop = false;
  export let disabled = false;
  export let tagColor = 'primary';
  export let tagSize = 'normal';
  export let hasError = false;
  export let placeholder = null;
  export let inputSize = 'normal';

  $: _disabled = disabled || (maxTags && tags.length >= maxTags);
  $: _tags_left = maxTags ? maxTags - tags.length : 0;

  $: addKeys.length && addKeys.forEach(key => setKeyName(key, addKeyNames));
  $: removeKeys.length && removeKeys.forEach(key => setKeyName(key, removeKeyNames));
  $: if(tagSize && sizeMap.indexOf(tagSize) != -1) {
      let index = sizeMap.indexOf(tagSize);
      tagRemoveButtonSize = sizeMap[index-1] || 'small';
    }

  $: setKeyName = (key, obj) => {
    let keyName = KeyMap[key];
    if(keyName) {
      let keyNameLowerCase = keyName.toLowerCase();
      keyName = keyName.charAt(0).toUpperCase() + keyNameLowerCase.slice(1);
      obj.push(keyName);
    }
  }

  function createByKey(e) {
    if(addKeys && addKeys.indexOf(e.keyCode) != -1) {
      e.preventDefault();
      addTag();
    }
  }

  function createByPaste(e) {
    if(!allowPaste) return;

    let data = '';

    if(window.clipboardData) {
      data = window.clipboardData.getData('Text');
    }

    if(e.clipboardData) {
      data = e.clipboardData.getData('text/plain');
    }

    if(data) {
      let chunks = data.split(splitWith).map(s => s.trim());
      if(chunks) {
        chunks.forEach(tag => {
          if(tags.length < maxTags){
            addTag(tag)
          }
        });
      }
    }
  }

  function createByDrop(e) {
    if(!allowDrop) return;

    e.preventDefault();

    let data = e.dataTransfer.getData("Text");
    if(data) {
      let chunks = data.split(splitWith).map(s => s.trim());
      if(chunks) {
        chunks.forEach(tag => {
          if(tags.length < maxTags){
            addTag(tag)
          }
        });
      }
    }
  }

  function removeByKey(e) {
    if(removeKeys && removeKeys.indexOf(e.keyCode) != -1 && tag === '') {
      tags.pop();
      tags = tags;

      dispatch('change', {tags});
    }
  }

  function removeByClick(i) {
    if(disabled) return;

    tags.splice(i, 1);
    tags = tags;

    dispatch('change', {tags});

    setTimeout(() => {
      tagInput && tagInput.focus();
    }, 10);
  }

  function addTag(_tag = '') {
    let newTag = _tag || tag;
    newTag = newTag.trim();

    if(!newTag || _disabled || (!allowDuplicates && tags.includes(newTag))) return;

    tags.push(newTag);
    tags = tags;
    tag = '';

    dispatch('change', {tags});
  }
</script>
