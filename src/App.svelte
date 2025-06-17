<script>
  import { onMount } from "svelte";
  import { API_URL } from "./lib/api";
  import NoteCard from "./components/NoteCard.svelte";
  import AddNoteForm from "./components/AddNoteForm.svelte";
  import EditNoteModal from "./components/EditNoteModal.svelte";
  import SearchBar from "./components/SearchBar.svelte";
  import ConfirmationDeleteModal from "./components/ConfirmationDeleteModal.svelte";
  import Pagination from "./components/Pagination.svelte";
 
  let notes = []; 
  let title = '';
  let content = '';
  let showForm = false;
  let searchTerm = '';
  let showConfirmModal = false;
  let noteToDelete = null;
  let currentPage = 1;
  const notesPerPage = 20;

  $: filteredNotes = notes
  .filter(note =>
    note.title.toLowerCase().includes(searchTerm.toLowerCase())
  )
  .sort((a, b) => {
    if (a.pinned && !b.pinned) return -1;
    if (!a.pinned && b.pinned) return 1;
    return new Date(b.createdAt) - new Date(a.createdAt);
  });

  $: totalPages = Math.ceil(filteredNotes.length / notesPerPage);

  $: paginatedNotes = filteredNotes.slice(
    (currentPage - 1) * notesPerPage,
    currentPage * notesPerPage
  );

  let isLoading = true;

  onMount(async () => {
    try {
      const response = await fetch('https://684f0b5bf0c9c9848d29ef4d.mockapi.io/api/notes');
      const data = await response.json();
      notes = data;
      isDark = localStorage.getItem('theme') === 'dark';
		  document.documentElement.classList.toggle('dark', isDark);
    } catch (err) {
      console.error('Failed to fetch notes', err);
    } finally {
      isLoading = false;
    }
  });

  //to add note
  async function createNote(noteTitle, noteContent) {
    if (!noteTitle || !noteContent) return;

    const noteStructure = {
      title: noteTitle,
      content: noteContent,
      createdAt: new Date().toISOString(),
      pinned: false
    };
    try {
      const response = await fetch(API_URL, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(noteStructure),
      });

      const savedNote = await response.json();
      notes = [savedNote, ...notes];
    } catch (err) {
      console.error('Error adding note:', err);
    }
  }

  async function togglingPin(note) {
    try {
      const response = await fetch(`${API_URL}/${note.id}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ ...note, pinned: !note.pinned })
      });
      const updated = await response.json();
      notes = notes.map(n => n.id === updated.id ? updated : n);
    } catch (err) {
      console.error('Error toggling pin:', err);
    }
  }


  //to delete the note
  function deleteConfirmation(note) {
    noteToDelete = note;
    showConfirmModal = true;
  }

  async function deleteConfirmedNote() {
    try {
      await fetch(`${API_URL}/${noteToDelete.id}`, {
        method: 'DELETE',
      });

      notes = notes.filter(note => note.id !== noteToDelete.id);
    } catch (err) {
      console.error('Error in deleting the note:', err);
    } finally {
      showConfirmModal = false;
      noteToDelete = null;
    }
  }

  //to edit the note
  let showEditModal = false;
  let editingNote = null;
  let isSaving = false;
  
  function editNote(note) {
    console.log("Edit triggered", note);
    editingNote = { ...note };
    showEditModal = true;
  }
  async function savedEditedNote() {
    isSaving = true;
    try {
      const response = await fetch(`${API_URL}/${editingNote.id}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          title: editingNote.title,
          content: editingNote.content,
          createdAt: editingNote.createdAt
        })
      });

      const updatedNote = await response.json();
      notes = notes.map(n => n.id === updatedNote.id ? updatedNote : n);
      showEditModal = false;
      editingNote = null;
    } catch (err) {
      console.error('Error in updating the note:', err);
    } finally {
      isSaving = false;
    }
  }
  let isDark = false;

  function toggleTheme() {
    isDark = !isDark;
    document.documentElement.classList.toggle("dark");
  }

</script>

<main class="w-full min-h-screen h-auto p-8 bg-background">
  <div class="flex flex-row justify-between">
    <div>
      <h1 class="text-3xl font-bold text-notesHeading">My Notes</h1>
      <p class="text-thoughts mb-6">Capture your thoughts and ideas</p>
    </div>
    <button on:click={toggleTheme} class="text-md mb-8 px-2 py-1 text-togglebuttontext cursor-pointer border-2 border-gray-400 rounded-full shadow-gray-100 bg-togglebutton rounded">
      {isDark ? '☀️ Light Mode' : '🌙 Dark Mode'}
    </button>
  </div>

  {#if showForm}
    <AddNoteForm
      title={title}
      content={content}
      setTitle={(val) => title = val}
      setContent={(val) => content = val}
      addNote={createNote}
      closeForm={() => showForm = false}
    />
  {/if}

  <SearchBar
    bind:searchTerm
    showForm={showForm}
    toggleForm={() => showForm = !showForm}
    onSearch={() => currentPage = 1}
  />

  {#if isLoading}
    <div class="flex flex-col items-center justify-center py-10 text-gray-600">
      <div class="animate-spin rounded-full h-10 w-10 border-t-2 border-b-2 border-blue-500 mb-3"></div>
      <p class="text-lg font-medium">Loading notes...</p>
    </div>
  {:else}
    {#if notes.length === 0}
      <p class="text-center text-nonotes mt-10">No notes available.</p>
    {:else}
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        {#each paginatedNotes as note}
          <NoteCard
            {note}
            on:edit={(e) => editNote(e.detail)}
            on:delete={(e) => deleteConfirmation(e.detail)}
            onPinToggle={togglingPin}
          />
        {/each}
      </div>
    {/if}
  {/if}

  {#if showEditModal && editingNote}
    <EditNoteModal
      {editingNote}
      note={editingNote}
      isSaving={isSaving}
      onClose={() => showEditModal = false}
      onSave={savedEditedNote}
    />
  {/if}

  <Pagination
    {currentPage}
    {totalPages}
    onNext={() => currentPage++}
    onPrev={() => currentPage--}
  />

  {#if showConfirmModal && noteToDelete}
  <ConfirmationDeleteModal
      note={noteToDelete}
      onCancel={() => showConfirmModal = false}
      onDelete={deleteConfirmedNote}
    />
  {/if}
</main>