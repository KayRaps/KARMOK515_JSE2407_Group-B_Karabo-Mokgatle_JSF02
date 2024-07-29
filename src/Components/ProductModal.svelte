<script>
    export let modalProduct;
  
    import { createEventDispatcher } from 'svelte';
  
    const dispatch = createEventDispatcher();
  
    function closeModal() {
      dispatch('closeModal');
    }
  </script>
  
  <div class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
    <div class="bg-white p-8 rounded-lg max-w-lg w-full">
      <img src="{modalProduct.image}" alt="{modalProduct.title}" class="w-full h-64 object-cover mb-4" />
      <h2 class="text-2xl font-bold mb-2">{modalProduct.title}</h2>
      <p class="text-gray-700">${modalProduct.price}</p>
      <p class="text-gray-500">{modalProduct.category}</p>
      <p class="text-gray-700 mt-4">{modalProduct.description}</p>
      <button
        on:click={closeModal}
        class="bg-blue-500 text-black hover:bg-white border p-2 rounded mt-4"
      >
        Close
      </button>
    </div>
  </div>
  