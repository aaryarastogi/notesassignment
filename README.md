# Notes App
A simple and fast Notes app built with **Svelte**, **Tailwind CSS**, and **MockAPI**. It supports creating, editing, deleting, and searching notes — with responsive design and dark mode support.

**Deployed Link** : https://quicknotex.netlify.app/

## How to run the app

1. First clone the repo : https://github.com/aaryarastogi/notesassignment.git
2. Go to your folder: cd svelte-notes-app
3. Install dependencies:  npm install 
4. Run the website: npm run dev
5. Visit the app: http://localhost:5173/

## Features:
1. CRUD (Create , Read , Update , Delete) operations on notes.
2. Pagination feature means only 20 notes visible on one page all the other notes you can see by clicking on previous or next button.
3. Searching a note by its title.
4. Sorting the notes on the of their creation time.
5. You can pin the notes as well to see that note at the top and can also unpin them.
6. While loading and updating notes there will be a loading spinner visible.

## TradeOffs
1. MockAPI is used as the backend to simplify the setup — no real database or auth integration.
2. Optimistic UI is not implemented; note updates wait for server confirmation.
3. Notes are sorted based on their creation timestamp (createdAt).
4. Basic error handling is included but not comprehensive.
5. Mobile responsiveness is prioritized over desktop-specific UX polish.

## What I'd do with more time
1. Add localStorage or Theme persistence for dark/light mode.
2. Improve error handling and add retry mechanisms for API failures.
3. Add pagination or virtual scrolling for better performance on large note sets.
4. Integrate user authentication (login/signup).
5. Add rich-text editor support (Markdown or WYSIWYG).
6. Write unit and integration tests for critical components.

## Tech Stack
1. Svelte
2. Mock API , Fetch API
3. Tailwind CSS
