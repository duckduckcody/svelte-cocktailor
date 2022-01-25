<script lang="ts">
  import { Ingredient, ingredients } from "../../data/ingredients.svelte";
  import { selectedIngredientsStore } from "../../store.svelte";

  let search: string = "";
  let results: Ingredient[] = [];
  let selectedIngredients: Ingredient[] = [];

  $: results = ingredients.filter((ingredient) =>
    ingredient.name.toLocaleLowerCase().includes(search.toLocaleLowerCase())
  );

  selectedIngredientsStore.subscribe((ingredients) => {
    selectedIngredients = ingredients;
  });

  const onInputBlur = () => {
    if (results.length === 0) {
      search = "";
    }
  };

  const onIngredientClick = (ingredient: Ingredient) => {
    selectedIngredientsStore.update((ingredients) => [
      ...ingredients,
      ingredient,
    ]);
    search = "";
  };

  const onIngredientTrash = (index: number) => {
    selectedIngredientsStore.update((ingredients) =>
      ingredients.slice(index, index)
    );
  };
</script>

<label>
  Find: <input type="text" bind:value={search} on:blur={onInputBlur} />
  <button on:click={() => (search = "")}>Clear</button>
</label>

{#if search}
  <div class="result-container">
    {#if results.length}
      {#each results as result (result.ingredientId)}
        <p
          class="result ingredient-result"
          on:click={() => onIngredientClick(result)}
        >
          {result.name}
        </p>
      {/each}
    {:else}
      <p class="result">No ingredients found</p>
    {/if}
  </div>
{/if}

{#each selectedIngredients as selectedIngredient, index (selectedIngredient.ingredientId)}
  <div class="ingredient-container">
    <div class="ingredient-card">
      <p>{selectedIngredient.name}</p>

      <span class="trash" on:click={() => onIngredientTrash(index)}>X</span>
    </div>
  </div>
{/each}

<style>
  label {
    display: flex;
    flex-flow: row nowrap;
    align-items: center;
    gap: 8px;
  }

  input {
    width: 100%;
    margin: 0;
    border: 1px solid black;
    border-radius: 0;
  }

  input:focus {
    outline: 1.1px solid black;
  }

  button {
    margin: 0;
    color: black;
    background: none;
    border: 1px solid black;
    border-radius: 5px;
    cursor: pointer;
  }

  .result-container {
    border: 1px solid black;
    border-top: none;
    margin-bottom: 13px;
  }

  .result {
    padding: 13px;
    margin: 0;
  }

  .ingredient-result:hover {
    background: black;
    color: white;
    cursor: pointer;
  }

  .ingredient-container {
    margin-top: 13px;
  }

  .ingredient-card {
    border: 1px solid black;
    padding: 0 13px;
    display: flex;
    flex-flow: row nowrap;
    align-items: center;
    justify-content: space-between;
  }

  .trash {
    font-size: 1.3em;
    cursor: pointer;
    padding: 8px 0 8px 8px;
  }
</style>
