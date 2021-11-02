<script lang="ts">
  import {flip} from 'svelte/animate';

  let list = [{name: 'Hanna'}, {name: 'Julia'}, {name: 'Ida'}, {name: 'Lena'}, {name: 'Philipp'}];
  let hovering = null;
  
  const dragstart = (event: DragEvent, i: number) => {
    event.dataTransfer.effectAllowed = 'move';
    event.dataTransfer.dropEffect = 'move';
    const start = i.toString();
    event.dataTransfer.setData('text/plain', start);
  }

  const drop = (event, target: number) => {
    event.dataTransfer.dropEffect = 'move'; 
    const start = parseInt(event.dataTransfer.getData("text/plain"));
    const newTracklist = list

    if (start < target) {
      newTracklist.splice(target + 1, 0, newTracklist[start]);
      newTracklist.splice(start, 1);
    } else {
      newTracklist.splice(target, 0, newTracklist[start]);
      newTracklist.splice(start + 1, 1);
    }
    list = newTracklist
    hovering = null;
  }
</script>

<div class="list">
{#each list as item, index (item.name)}
  <div 
    class="list-item"
    animate:flip
    draggable={true}
    on:dragstart={event => dragstart(event, index)}
    on:drop|preventDefault={event => drop(event, index)}
    on:dragover|preventDefault
    on:dragenter={() => hovering = index}
    class:is-active={hovering === index}>
    {item.name}
  </div>
{/each}
</div>

<style>
  .list {
    background-color: white;
    border-radius: 4px;
    box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1), 0 0 0 1px rgba(10, 10, 10, 0.1);
  }

  .list-item {
    display: block;
    padding: 0.5em 1em;
  }

  .list-item:not(:last-child) {
    border-bottom: 1px solid #dbdbdb;
  }

  .list-item.is-active {
    background-color: #3273dc;
    color: #fff;
  }
</style>