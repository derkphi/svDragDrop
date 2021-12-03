<script lang="ts">
  import {flip} from 'svelte/animate';
  export let title : string;
  export let items : {name: string}[] = [];

  let start = undefined;
  let hovering = null;
  
  const dragstart = (event: DragEvent, i: number) => {
    event.dataTransfer.effectAllowed = 'move';
    event.dataTransfer.dropEffect = 'move';
    start = i;
    event.dataTransfer.setData('list/item', JSON.stringify(items[i]));
  }

  const dragend = () => {
    hovering = null;
    start = undefined;
  }

  const drop = (event, target: number) => {
    console.log("Drop to: " + title + " " + target);
    event.dataTransfer.dropEffect = 'move'; 
    const item = JSON.parse(event.dataTransfer.getData("list/item"));
    if (item) {
      const newTracklist = items

      if (start < target) {
        newTracklist.splice(target + 1, 0, newTracklist[start]);
        newTracklist.splice(start, 1);
      } else {
        newTracklist.splice(target, 0, newTracklist[start]);
        newTracklist.splice(start + 1, 1);
      }
      items = newTracklist
    }
  }
</script>

<div class="list">
  <div class="title">{title}</div>
  {#each items as item, index (item.name)}
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
  <div 
    class="list-fill"
    draggable={true}
    on:drop|preventDefault={event => drop(event, items.length)}
    on:dragover|preventDefault={() => hovering = items.length}
    on:dragenter|preventDefault
    on:dragleave={() => hovering = null}
    class:is-active={items.length > 0 ? hovering >= items.length-1 : hovering === items.length}
    class:is={start !== undefined}/>
</div>

<style>
  .list {
    background-color: white;
    border-radius: 4px;
    box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1), 0 0 0 1px rgba(10, 10, 10, 0.1);
    padding: 5px;
    margin: 10px;
    width: 140px;
    min-width: 100px;
    height: 250px;
    display: flex;
    flex-flow: column;
  }

  .title {
    text-align: center;
    font-size: 20px;
    padding: 5px;
    background-color:lavender;
    margin-bottom: 5px;
    flex: 0 1 auto;
  }

  .list-fill {
    flex: 1 1 auto;
  }

  .list-fill.is {
    border: 1px solid orange;
  }

  .list-fill.is-active {
    border: 1px solid blue;
  }

  .list-item {
    padding: 0.5em 1em;
    border: solid #dbdbdb; border-width: 1px 0 1px 0;
    flex: 0 1 auto;
  }

  .list-item:not(:last-child) {
    border-bottom: 1px solid white;
  }

  .list-item.is-upper:not(:nth-last-child(2)) {
    border-color: transparent;
    border-bottom: 1px solid orange;
  }

  .list-item.is-active-upper:not(:nth-last-child(2)) {
    border-color: transparent;
    border-bottom: 1px solid blue;
  }

  .list-item.is-lower:not(:nth-last-child(2)) {
    border-color: transparent;
    border-top: 1px solid orange;
  }

  .list-item.is-active-lower:not(:nth-last-child(2)) {
    border-color: transparent;
    border-top: 1px solid blue;
  }
</style>