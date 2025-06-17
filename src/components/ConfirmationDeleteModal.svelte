<script>
  export let note;
  export let onCancel;
  export let onDelete;
</script>

<div class="fixed inset-0 bg-transparent backdrop-blur-xl flex items-center justify-center z-50">
  <div class="bg-notesbackground rounded-xl shadow-xl p-6 w-[90%] max-w-md">
    <h2 class="text-lg font-semibold mb-4 text-searchbartext text-center">Are you sure you want to delete this note?</h2>
    <p class="mb-4 text-notescontent text-center">"{note?.title}" will be permanently removed.</p>
    <div class="flex justify-end gap-3">
      <button
        on:click={onCancel}
        class="px-4 py-2 bg-gray-200 rounded hover:bg-gray-300 cursor-pointer"
      >
        Cancel
      </button>
      <button
        on:click={onDelete}
        class="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600 cursor-pointer"
      >
        Delete
      </button>
    </div>
  </div>
</div>
