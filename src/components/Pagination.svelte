<script>
  export let currentPage;
  export let totalPages;
  export let onNext;
  export let onPrev;
</script>

<div class="flex justify-center items-center mt-6 gap-4">
  <button
    on:click={onPrev}
    class="px-4 py-2 bg-gray-300 rounded hover:bg-gray-400 disabled:opacity-50 cursor-pointer"
    disabled={currentPage === 1}
  >
    Previous
  </button>

  <span class="text-notescontent font-semibold">Page {currentPage} of {totalPages}</span>

  <button
    on:click={onNext}
    class="px-4 py-2 bg-gray-300 rounded hover:bg-gray-400 disabled:opacity-50 cursor-pointer"
    disabled={currentPage === totalPages}
  >
    Next
  </button>
</div>
