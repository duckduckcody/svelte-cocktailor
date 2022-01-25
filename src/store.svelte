<script context="module" lang="ts">
  import { writable } from "svelte/store";
  import type { Ingredient } from "./data/ingredients.svelte";

  export const selectedIngredientsStore = writable<Ingredient[]>([]);
</script>
