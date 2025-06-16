<script>
  import { createEventDispatcher } from 'svelte';
  export let note;
  export let onPinToggle;

  const dispatch = createEventDispatcher();
</script>

<div class="relative bg-white p-4 rounded-xl shadow-md shadow-blue-200 flex flex-col justify-between items-start transform transition-transform duration-200 hover:-translate-y-1 hover:shadow-lg cursor-pointer">
  <div>
    <h2 class="text-xl font-semibold capitalize mb-2">{note.title}</h2>
    <p class="text-gray-700 mb-2">{note.content}</p>
  </div>
  <button
    on:click={() => onPinToggle(note)}
    class="absolute top-2 right-2 text-gray-600 hover:text-gray-800"
    title={note.pinned ? 'Unpin' : 'Pin'}
  >
    📌 {note.pinned ? 'Unpin' : 'Pin'}
  </button>

  <div class="flex gap-2 py-2">
    <button
      on:click={() => dispatch('edit', note)}
      class="border border-gray-400 text-gray-800 px-2 py-1 rounded hover:bg-gray-200 cursor-pointer"
    >
      ✎ Edit
    </button>
    <button
      on:click={() => dispatch('delete', note)}
      class="border border-red-400 text-red-400 px-2 py-1 rounded hover:bg-red-100 cursor-pointer"
    >
       <i class="fa-solid fa-trash" style="color:red;"></i>
      Delete
    </button>
  </div>
</div>
