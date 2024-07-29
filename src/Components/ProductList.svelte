<script>
    import { createEventDispatcher } from 'svelte';
    import ProductCard from './ProductCard.svelte';
    import ProductFilter from './ProductFilter.svelte';
  
    export let categories = [];
    export let filteredProducts = [];
    export let searchQuery = "";
    export let selectedCategory = "";
    export let sortOrder = "";
    export let loading = false;
  
    const dispatch = createEventDispatcher();
  
    function handleFilter() {
      dispatch('filterProducts');
    }
  
    function handleSort() {
      dispatch('sortProducts');
    }
  </script>
  
  <ProductFilter
    {categories}
    bind:searchQuery
    bind:selectedCategory
    bind:sortOrder
    on:filterProducts={handleFilter}
    on:sortProducts={handleSort}
  />
  
  <div class="loading text-center text-2xl" class:hidden="{!loading}">
    Loading...
  </div>
  
  <div class="grid grid-cols-1 hidden:grid-cols-2 lg:grid-cols-4 gap-6 hover:shadow-lg">
    {#each filteredProducts as product (product.id)}
      <ProductCard
        {product}
        on:openModal={() => dispatch('openModal', product)}
        on:toggleFavorite={() => dispatch('toggleFavorite', product.id)}
      />
    {/each}
  </div>
  