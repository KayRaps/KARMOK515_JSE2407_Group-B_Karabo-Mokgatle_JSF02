<script>
  import { onMount } from 'svelte';
  import NavBar from './Components/NavBar.svelte';
  import ProductList from './Components/ProductList.svelte';
  import ProductModal from './Components/ProductModal.svelte';
  import ProductCard from './Components/ProductCard.svelte';

  
  let products = [];
  let categories = [];
  let filteredProducts = [];
  let searchQuery = "";
  let selectedCategory = "";
  let sortOrder = "";
  let favorites = JSON.parse(localStorage.getItem("favorites")) || [];
  let showModal = false;
  let modalProduct = {};
  let loading = true;

  onMount(async () => {
    await fetchCategories();
    await fetchProducts();
  });

  async function fetchProducts() {
    loading = true;
    const response = await fetch("https://fakestoreapi.com/products");
    const data = await response.json();
    products = data;
    filteredProducts = data;
    loading = false;
  }

  async function fetchCategories() {
    const response = await fetch("https://fakestoreapi.com/products/categories");
    categories = await response.json();
  }

  function openModal(product) {
    loading = true;
    fetch(`https://fakestoreapi.com/products/${product.id}`)
      .then((response) => response.json())
      .then((data) => {
        modalProduct = data;
        showModal = true;
        loading = false;
      });
  }

  function closeModal() {
    showModal = false;
  }

  function toggleFavorite(productId) {
    if (favorites.includes(productId)) {
      favorites = favorites.filter((id) => id !== productId);
    } else {
      favorites.push(productId);
    }
    localStorage.setItem("favorites", JSON.stringify(favorites));
  }

  function isFavorite(productId) {
    return favorites.includes(productId);
  }

  function filterProducts() {
    loading = true;
    filteredProducts = products.filter((product) => {
      const matchesSearch = product.title
        .toLowerCase()
        .includes(searchQuery.toLowerCase());
      const matchesCategory =
        selectedCategory === "" || product.category === selectedCategory;
      return matchesSearch && matchesCategory;
    });
    sortProducts();
    loading = false;
  }

  function sortProducts() {
    if (sortOrder) {
      filteredProducts.sort((a, b) => {
        if (sortOrder === "asc") {
          return a.price - b.price;
        } else if (sortOrder === "desc") {
          return b.price - a.price;
        }
        return 0;
      });
    }
  }
</script>

<NavBar />
<div class="container mx-auto p-6">
  <ProductList
    {categories}
    {filteredProducts}
    {searchQuery}
    {selectedCategory}
    {sortOrder}
    {loading}
    on:openModal={openModal}
    on:toggleFavorite={toggleFavorite}
    on:filterProducts={filterProducts}
    on:sortProducts={sortProducts}
  />

  {#if showModal}
    <ProductModal {modalProduct} on:closeModal={closeModal} />
  {/if}
</div>

<style>
  .container {
    max-width: 1200px;
    margin: 0 auto;
  }
</style>
