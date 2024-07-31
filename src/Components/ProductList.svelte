<script>
  // @ts-nocheck
  
  import { Link } from 'svelte-routing';
  
  export let products = [];
  
  let categories = [];
  let selectedCategory = '';
  let sortOrder = '';
  
  const fetchCategories = async () => {
    const res = await fetch('https://fakestoreapi.com/products/categories');
    categories = await res.json();
  };
  
  const handleCategoryChange = (/** @type {{ target: { value: string; }; }} */ event) => {
    selectedCategory = event.target.value;
  };
  
  const handleSortChange = (/** @type {{ target: { value: string; }; }} */ event) => {
    sortOrder = event.target.value;
  };
  
  $: filteredProducts = products
    .filter(product => selectedCategory ? product.category === selectedCategory : true)
    .sort((a, b) => sortOrder === 'low-to-high' ? a.price - b.price : sortOrder === 'high-to-low' ? b.price - a.price : 0);
  
  fetchCategories();
  </script>
  
  <style>
  .controls-container {
    display: flex;
    justify-content: center; /* Center horizontally */
    align-items: center; /* Center vertically */
    margin: 1rem 0; /* Add some margin */
    padding: 1rem 2rem; /* Add padding */
    border: 2px solid #090808; /* Add border */
    background-color: #f0f0f0; /* Light background color */
    border-radius: 8px; /* Rounded corners */
  }

  .select-dropdown {
    margin: 0 1rem; /* Spacing between dropdowns */
    padding: 0.5rem 1rem; /* Padding inside the dropdown */
    border: 1px solid #ccc; /* Border for dropdown */
    border-radius: 4px; /* Rounded corners for dropdown */
    background-color: #fff; /* Background color for dropdown */
    cursor: pointer;
  }

     .product-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}
    .product-card {
      border: 3px solid #090808;
      padding: 1rem;
      border-radius: 8px;
      cursor: pointer;
      width: 200px;
      background-color: grey;
    }
  </style>
  
  <div class="controls-container">
  <select on:change={handleCategoryChange}>
    <option value=''>All Categories</option>
    {#each categories as category}
      <option value={category}>{category}</option>
    {/each}
  </select>
  
  <select on:change={handleSortChange}>
    <option value=''>Default</option>
    <option value='low-to-high'>Price: Low to High</option>
    <option value='high-to-low'>Price: High to Low</option>
  </select>
  </div>
  
  <div class="product-list">
    {#each filteredProducts as product}
      <Link to="/product/{product.id}">
        <div class="product-card">
          <img src={product.image} alt={product.title} class="w-40 max-w-[300px] h-auto object-contain rounded-lg" />
          <h2>{product.title}</h2>
          <p>${product.price}</p>
          <p>Category: {product.category}</p>
          <p>Ratings: {product.rating.rate}</p>
          <p>Reviews: {product.rating.count}</p>
        </div>
      </Link>
    {/each}
  </div>
  