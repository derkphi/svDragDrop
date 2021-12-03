<script lang="ts">
  import ListBox from './ListBox.svelte'

  let list = [{name: 'Hanna'}, {name: 'Julia'}, {name: 'Ida'}, {name: 'Lena'}, {name: 'Philipp'}];
  
</script>

<div class="container">
  <ListBox items={list} title="Fist Box"/>
  <ListBox title="Second Box"/>
</div>

<style>
  .container {
    display: flex;
    flex-direction: row;
  }
</style>