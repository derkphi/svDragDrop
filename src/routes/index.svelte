<script lang="ts">
  import {flip} from 'svelte/animate';

  let list = [{name: 'Hanna'}, {name: 'Julia'}, {name: 'Ida'}, {name: 'Lena'}, {name: 'Philipp'}];
  let start = undefined;
  let hovering = null;
  
  const dragstart = (event: DragEvent, i: number) => {
    event.dataTransfer.effectAllowed = 'move';
    event.dataTransfer.dropEffect = 'move';
    start = i;
    //event.dataTransfer.setData('text/plain', i.toString());
  }

  const dragend = () => {
    hovering = null;
    start = undefined;
  }

  const drop = (event, target: number) => {
    event.dataTransfer.dropEffect = 'move'; 
    //const start = parseInt(event.dataTransfer.getData("text/plain"));
    const newTracklist = list

    if (start < target) {
      newTracklist.splice(target + 1, 0, newTracklist[start]);
      newTracklist.splice(start, 1);
    } else {
      newTracklist.splice(target, 0, newTracklist[start]);
      newTracklist.splice(start + 1, 1);
    }
    list = newTracklist
  }
</script>

<div class="container">
<div class="list">
{#each list as item, index (item.name)}
  <div 
    class="list-item"
    animate:flip
    draggable={true}
    on:dragstart={event => dragstart(event, index)}
    on:dragend={event => dragend()}
    on:drop|preventDefault={event => drop(event, index)}
    on:dragover|preventDefault={() => hovering = index}
    on:dragenter|preventDefault
    on:dragleave={() => hovering = null}
    class:is-active-upper={hovering === index && start < index}
    class:is-upper={start !== undefined && start < index}
    class:is-active-lower={hovering === index && start > index}
    class:is-lower={start !== undefined && start > index}>
    {item.name}
  </div>
{/each}
</div>
<div class="list"></div>
</div>

<style>
  .container {
    display: flex;
    flex-direction: row;
  }

  .list {
    background-color: white;
    border-radius: 4px;
    box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1), 0 0 0 1px rgba(10, 10, 10, 0.1);
    padding: 5px;
    margin: 10px;
    width: 100px;
    min-width: 100px;
    height: 200px;
  }

  .list-item {
    display: block;
    padding: 0.5em 1em;
    border: solid #dbdbdb; border-width: 1px 0 1px 0;
  }

  .list-item:not(:last-child) {
    border-bottom: 1px solid white;
  }

  .list-item.is-upper {
    border-color: transparent;
    border-bottom: 1px solid orange;
  }

  .list-item.is-active-upper {
    border-color: transparent;
    border-bottom: 1px solid blue;
  }

  .list-item.is-lower {
    border-color: transparent;
    border-top: 1px solid orange;
  }

  .list-item.is-active-lower {
    border-color: transparent;
    border-top: 1px solid blue;
  }
</style>