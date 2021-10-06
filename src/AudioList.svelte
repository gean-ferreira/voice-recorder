<script>
  import { Icon } from "sveltestrap";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let title;
  export let audio;
</script>

<div class="library">
  <span class="title">{title}</span>
  <audio controls src={audio} />
  <button class="trash" on:click={() => dispatch("deleterecording")}
    ><Icon name="trash-fill" /></button
  >
</div>

<style>
  .title {
    text-align: center;
    width: 200px;
  }
  .library {
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    padding-bottom: 0.5rem;
  }
  .trash {
    padding: 0.2rem;
    border: none;
    background-color: burlywood;
  }
</style>
