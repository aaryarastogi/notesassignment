<script>
  export let searchTerm = '';
  export let onSearch;
  export let showForm = false;
  export let toggleForm;
</script>

<div class="flex justify-between items-center mb-6">
  <input
    type="text"
    bind:value={searchTerm}
    on:input={onSearch}
    placeholder="Search notes..."
    class="w-full p-2 border border-gray-300 rounded mr-4 "
  />
  <button
    on:click={toggleForm}
    class="md:w-32 w-48 bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 cursor-pointer dark: bg-black"
  >
    {showForm ? 'Close Form' : 'New Note'}
  </button>
</div>
