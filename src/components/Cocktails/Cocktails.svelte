<script lang="ts">
  import { Cocktail, cocktails } from "../../data/cocktails.svelte";
  import { ingredients } from "../../data/ingredients.svelte";

  import { selectedIngredientsStore } from "../../store.svelte";

  let matchedCocktails: Cocktail[] = [];
  let ingredientIds: number[] = [];

  selectedIngredientsStore.subscribe((ingredients) => {
    ingredientIds = ingredients.map((ingredient) => ingredient.ingredientId);
    matchedCocktails = cocktails.filter((cocktail) =>
      cocktail.ingredients.some((ingredient) =>
        ingredientIds.includes(ingredient.ingredientId)
      )
    );
  });
</script>

<div class="cocktail-container">
  {#each matchedCocktails as matchedCocktail (matchedCocktail.cocktailId)}
    <div class="cocktail-card">
      <div>
        <p class="cocktail-name">{matchedCocktail.name}</p>
        <div class="ingredient-list">
          {#each matchedCocktail.ingredients as ingredient (ingredient.ingredientId)}
            <div
              class="ingredient-item"
              class:ingredient-highlight={ingredientIds.includes(
                ingredient.ingredientId
              )}
            >
              <span>{ingredient.amount} -&nbsp;</span>
              <span>
                {ingredients
                  .find(
                    (ingredientData) =>
                      ingredientData.ingredientId === ingredient.ingredientId
                  )
                  .name.trim()}
              </span>
            </div>
          {/each}
        </div>
      </div>
      <div>
        {matchedCocktail.method}
      </div>
    </div>
  {/each}
</div>

<style>
  .cocktail-container {
    display: flex;
    flex-flow: column nowrap;
    gap: 8px;
  }

  .cocktail-card {
    border: 1px solid black;
    padding: 13px;
    display: flex;
    flex-flow: row nowrap;
    justify-content: space-between;
    gap: 48px;
  }

  .cocktail-name {
    margin: 0 0 8px 0;
  }

  .ingredient-list {
    display: flex;
    flex-flow: column nowrap;
  }

  .ingredient-item {
    display: flex;
    flex-flow: row nowrap;
  }

  .ingredient-highlight {
    font-weight: bold;
  }
</style>
