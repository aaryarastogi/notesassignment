<script>
  import { createEventDispatcher } from 'svelte';
  export let note;
  export let onPinToggle;

  const dispatch = createEventDispatcher();
</script>

<div class="relative bg-notesbackground border-1 border-notesborder p-4 rounded-xl shadow-md shadow-notesshadow flex flex-col justify-between items-start transform transition-transform duration-200 hover:-translate-y-1 hover:shadow-lg cursor-pointer">
  <div>
    <h2 class="text-xl font-semibold capitalize mb-2 text-notestitle">{note.title}</h2>
    <p class="text-notescontent mb-2">{note.content}</p>
  </div>
  <button
    on:click={() => onPinToggle(note)}
    class="absolute top-2 right-2 text-pinbuttontext hover:text-hoveredpinbuttontext"
    title={note.pinned ? 'Unpin' : 'Pin'}
  >
    📌 {note.pinned ? 'Unpin' : 'Pin'}
  </button>

  <div class="flex gap-2 py-2">
    <button
      on:click={() => dispatch('edit', note)}
      class="border border-gray-400 text-editbuttontext px-2 py-1 rounded bg-editbuttonbackground hover:bg-hoverededitbuttonbackground cursor-pointer"
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
