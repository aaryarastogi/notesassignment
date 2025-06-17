<script>
  export let note;
  export let onClose;
  export let onSave;
  export let isSaving;
</script>

<div class="fixed inset-0 bg-transparent backdrop-blur-xl flex items-center justify-center z-50">
  <div class="bg-notesbackground rounded-lg shadow-lg w-full max-w-md p-6 relative">
    <button
      class="absolute top-2 right-2 text-gray-500 hover:text-gray-800 text-xl cursor-pointer"
      on:click={onClose}
    >
      ✕
    </button>
    <h2 class="text-xl font-semibold mb-4 text-notesHeading text-center">Edit Note</h2>

    <input
      type="text"
      bind:value={note.title}
      placeholder="Edit title"
      class="w-full mb-3 p-2 border border-searchbarborder rounded text-searchbartext"
    />
    <textarea
      bind:value={note.content}
      placeholder="Edit content"
      class="w-full mb-3 p-2 border border-searchbarborder rounded text-searchbartext"
    ></textarea>

    <button
      class="w-full bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 cursor-pointer"
      on:click={onSave}
      disabled={isSaving}
    >
      {#if isSaving}
        <span class="border-2 border-white border-t-blue-500 rounded-full w-4 h-4 inline-block animate-spin mr-2"></span>
      {:else}
        Save
      {/if}
    </button>
  </div>
</div>
