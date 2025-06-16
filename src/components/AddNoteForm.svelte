<script>
  export let title;
  export let content;
  export let setTitle;
  export let setContent;
  export let addNote;
  export let closeForm;

  let isSaving = false;

  async function handleAdd() {
    if (!title || !content) return;

    isSaving = true;
    await addNote(title, content);

    setTitle('');
    setContent('');
    isSaving = false;

    closeForm();
  }

</script>


<div class="fixed inset-0 bg-transparent backdrop-blur-xl flex items-center justify-center z-50">
  <div class="bg-white shadow-lg rounded-lg w-full max-w-md p-6 relative">
    <button
      class="absolute top-2 right-2 text-gray-500 hover:text-gray-800 text-xl cursor-pointer"
      on:click={closeForm}
      title="Close"
    >
      ✕
    </button>
    <h1 class="font-semibold text-2xl text-center mb-4">Add New Note</h1>

    <input
      type="text"
      bind:value={title}
      placeholder="Title"
      class="w-full mb-3 p-2 border border-gray-300 rounded text-black"
    />
    <textarea
      bind:value={content}
      placeholder="Content"
      class="w-full mb-3 p-2 border border-gray-300 rounded text-black"
    ></textarea>
    <button
      class="w-full bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 cursor-pointer flex justify-center items-center"
      on:click={handleAdd}
      disabled={isSaving}
    >
      {#if isSaving}
        <span class="border-2 border-white border-t-blue-500 rounded-full w-4 h-4 animate-spin mr-2"></span>
        Saving...
      {:else}
        Add Note
      {/if}
    </button>
  </div>
</div>
