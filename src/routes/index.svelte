<script lang="ts">
  function onDrop(event) {
    console.log("Dropped ", event.dataTransfer.getData("li"));

  }

  function onDrag(event) {
    event.dataTransfer.setData("li", event.target)
  }
</script>

<ul on:drop|preventDefault={onDrop}>
  <li draggable="true" on:dragover|preventDefault on:dragstart={onDrag}>List Item 1</li>
  <li draggable="true" on:dragover|preventDefault on:dragstart={onDrag}>List Item 2</li>
  <li draggable="true" on:dragover|preventDefault on:dragstart={onDrag}>List Item 3</li>
  <li draggable="true" on:dragover|preventDefault on:dragstart={onDrag}>List Item 4</li>
  <li draggable="true" on:dragover|preventDefault on:dragstart={onDrag}>List Item 5</li>
</ul>

<style>
  li {
  cursor:move;
  display:block;
  padding:20px 10px;
  background:white;
  border-bottom:solid 1px gray;
}
</style>