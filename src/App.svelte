<script>
  // @ts-ignore
  // @ts-ignore
  import { Router, Route } from 'svelte-routing';
  import Header from './Components/Header.svelte';
  import ProductList from './Components/ProductList.svelte';
  import ProductDetail from './Components/ProductDetail.svelte';
  import { onMount } from 'svelte';

  let products = []; // Assume you have a way to fetch and pass products to ProductList

  // Fetch products (for demonstration purposes)
  const fetchProducts = async () => {
    const res = await fetch('https://fakestoreapi.com/products');
    products = await res.json();
  };

  onMount(() => {
    fetchProducts();
  });
</script>

<Router>
  <Header />
  <div class="container">
    <Route path="/" let:params>
      <ProductList {products} />
    </Route>
    <Route path="/product/:id" let:params>
      <ProductDetail {params} />
    </Route>
  </div>
</Router>
