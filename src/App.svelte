<script lang="ts">
  import Cocktails from "./components/Cocktails/Cocktails.svelte";
  import Ingredients from "./components/Ingredients/Ingredients.svelte";

  let selected: string = "Ingredients";
</script>

<main>
  <p><b>Cocktailor</b></p>

  <div>
    <p
      class="selectable"
      class:selected={selected === "Ingredients"}
      on:click={() => (selected = "Ingredients")}
    >
      Ingredients
    </p>
    <p
      class="selectable"
      class:selected={selected === "Cocktails"}
      on:click={() => (selected = "Cocktails")}
    >
      Cocktails
    </p>
  </div>

  <div class={selected === "Ingredients" ? "shown" : "hidden"}>
    <Ingredients />
  </div>

  <div class={selected === "Cocktails" ? "shown" : "hidden"}>
    <Cocktails />
  </div>
</main>

<style>
  :global(p) {
    margin: 13px 0;
  }
  main {
    font-family: monospace;
    margin: 40px;
  }

  div {
    display: flex;
    flex-flow: row nowrap;
    gap: 16px;
  }

  .selectable {
    cursor: pointer;
  }

  .selected {
    text-decoration: underline;
  }

  .shown {
    display: block;
  }

  .hidden {
    display: none;
  }
</style>
