<!-- ProductCard.svelte -->
<script>
    export let product;
    export let isFavorite;
    export let toggleFavorite;
    export let openModal;
  
    function handleOpenModal() {
      openModal(product);
    }
  
    function handleToggleFavorite() {
      toggleFavorite(product.id);
    }
  </script>
  
  <div
    class="border p-4 rounded-lg shadow hover:shadow-lg bg-white transition-shadow duration-600 text-center"
    on:click="{handleOpenModal}"
  >
    <img
      src="{product.image}"
      alt="{product.title}"
      class="w-320px h-48 object-cover mb-4"
    />
    <h3 class="text-lg font-bold">{product.title}</h3>
    <p class="text-gray-700">${product.price}</p>
    <p class="text-gray-500">{product.category}</p>
    <p class="text-gray-700 mt-3 line-clamp-2">
      {product.description}
    </p>
    <button
      on:click|stopPropagation="{handleToggleFavorite}"
      class="bg-blue-500 text-black hover:bg-white border p-2 rounded mt-4"
    >
      {#if isFavorite}
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="currentColor"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="file: h-5 w-5 stroke-white inline-block mr-2"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M11.48 3.499a.75.75 0 011.04 0l2.194 2.194a4.5 4.5 0 015.45 5.45L13.334 20.025a2.25 2.25 0 01-3.184 0L4.836 14.71a4.5 4.5 0 015.45-5.45l1.194-1.194a.75.75 0 010-1.06l2.194-2.194a.75.75 0 011.06 0l1.194 1.194z"
          />
        </svg>
        Unfavorite
      {:else}
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="file: h-5 w-5 stroke-white inline-block mr-2"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M12 3.75l2.064 2.064a4.5 4.5 0 016.35 6.35L12 20.229l-8.414-8.414a4.5 4.5 0 016.35-6.35L12 3.75zm0 0l-1.194-1.194a.75.75 0 00-1.06 0L7.5 3.75m4.5-1.5L8.25 3.75m1.5-1.5L5.564 3.75M12 20.229L6.15 14.379m5.85 5.85l5.85-5.85m0 0l5.85-5.85"
          />
        </svg>
        Favorite
      {/if}
    </button>
  </div>
  
  <style>
    .line-clamp-2 {
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }
  </style>
  