<script>
  import { onMount } from "svelte";
  import { API_URL } from "./lib/api";
  import NoteCard from "./components/NoteCard.svelte";
    import AddNoteForm from "./components/AddNoteForm.svelte";
    import EditNoteModal from "./components/EditNoteModal.svelte";
    import DeleteConfirmModal from "./components/DeleteConfirmModal.svelte";
    import PaginationControls from "./components/PaginationControls.svelte";
    import SearchBar from "./components/SearchBar.svelte";

  let notes = [];
  let title = '';
  let content = '';
  let showForm = false;
  let searchTerm = '';
  let showConfirmModal = false;
  let noteToDelete = null;
  let currentPage = 1;
  const notesPerPage = 20;

  $: filteredNotes = notes.filter(note =>
    note.title.toLowerCase().includes(searchTerm.toLowerCase()) 
  );

  $: totalPages = Math.ceil(filteredNotes.length / notesPerPage);

  $: paginatedNotes = filteredNotes.slice(
    (currentPage - 1) * notesPerPage,
    currentPage * notesPerPage
  );

  onMount(async () => {
    await fetchNotes();
  });

  async function fetchNotes() {
    try {
      const res = await fetch(API_URL);
      const data = await res.json();
      notes = data.slice().sort((a, b) => {
        return new Date(b.createdAt).getTime() - new Date(a.createdAt).getTime();
      });
    } catch (err) {
      console.error('Error fetching notes:', err);
    }
  }

  //to add note
  async function addNote(noteTitle, noteContent) {
    if (!noteTitle || !noteContent) return;

    const noteData = {
      title: noteTitle,
      content: noteContent,
      createdAt: new Date().toISOString()
    };

    try {
      const res = await fetch(API_URL, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(noteData),
      });

      const savedNote = await res.json();
      notes = [savedNote, ...notes];
    } catch (err) {
      console.error('Error adding note:', err);
    }
  }

  //to delete the note
  function confirmDelete(note) {
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
      console.error('Error deleting note:', err);
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
  async function saveModalEdit() {
    isSaving = true;
    try {
      const res = await fetch(`${API_URL}/${editingNote.id}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          title: editingNote.title,
          content: editingNote.content,
          createdAt: editingNote.createdAt
        })
      });

      const updatedNote = await res.json();
      notes = notes.map(n => n.id === updatedNote.id ? updatedNote : n);
      showEditModal = false;
      editingNote = null;
    } catch (err) {
      console.error('Error updating note:', err);
    } finally {
      isSaving = false;
    }
  }
</script>

<main class="w-full min-h-screen h-auto p-8 bg-[#E3EAFF]">
  <h1 class="text-3xl font-bold">My Notes</h1>
  <p class="text-gray-700 mb-6">Capture your thoughts and ideas</p>

  <!-- Note Form -->
  {#if showForm}
    <AddNoteForm
      title={title}
      content={content}
      setTitle={(val) => title = val}
      setContent={(val) => content = val}
      addNote={addNote}
      closeForm={() => showForm = false}
    />
  {/if}


  <!-- search bar -->
  <SearchBar
    bind:searchTerm
    showForm={showForm}
    toggleForm={() => showForm = !showForm}
    onSearch={() => currentPage = 1}
  />


  <!-- Notes List -->
  <div class="grid md:grid-cols-4 gap-4">
   {#each paginatedNotes as note}
      <NoteCard
        {note}
        on:edit={(e) => editNote(e.detail)}
        on:delete={(e) => confirmDelete(e.detail)}
      />
    {/each}
  </div>

  {#if showEditModal && editingNote}
    <EditNoteModal
      {editingNote}
      note={editingNote}
      isSaving={isSaving}
      onClose={() => showEditModal = false}
      onSave={saveModalEdit}
    />
  {/if}

  <PaginationControls
    {currentPage}
    {totalPages}
    onNext={() => currentPage++}
    onPrev={() => currentPage--}
  />


  <!-- confirmation modal -->
  {#if showConfirmModal && noteToDelete}
  <DeleteConfirmModal
      note={noteToDelete}
      onCancel={() => showConfirmModal = false}
      onDelete={deleteConfirmedNote}
    />
  {/if}

</main>