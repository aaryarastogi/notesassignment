<script>
  import { createEventDispatcher } from 'svelte';
  export let note;

  const dispatch = createEventDispatcher();
</script>

<div class="bg-white p-4 rounded-xl shadow-md shadow-blue-200 flex flex-col justify-between items-start">
  <div>
    <h2 class="text-xl font-semibold">{note.title}</h2>
    <p class="text-gray-700">{note.content}</p>
  </div>
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
