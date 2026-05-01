# Collaborative Bulletin Board

Build a real-time shared bulletin board where anyone can post, edit, and remove sticker notes on a shared canvas. No accounts required. Every visitor who opens the page sees the same board and can interact with it immediately.

## Stack

- **Frontend**: Pure React (port **3000**)
- **Backend**: Django (port **3001**)
- **Persistence**: MySQL at port **3306**, schema **`notes`**
- **Real-time**: WebSockets

## The Board

The board fills the viewport and acts as the shared surface where all notes live. Users interact with it by clicking directly on the canvas to create notes, or clicking an existing note to edit or remove it.

When the page first loads, all notes stored in the database are fetched and rendered in their saved positions. The board looks identical to every connected user at any moment.

Clicking on any **empty area** of the board creates a new sticker note at that position. The note appears immediately on the board for all connected users. Notes are positioned absolutely on the board using the coordinates where the user clicked.

## Notes

Each note is a small card rendered at a fixed position on the board. A note has a single text field. When a user clicks on a note, it becomes the **active** note:

- A text cursor appears inside the note's text field, allowing the content to be modified.
- A **Delete** button becomes visible for the active note.
- Once the user starts modifying the text, a **Save** button becomes visible for that note.

Clicking **Save** persists the updated text and broadcasts the change to all connected users. Clicking **Delete** removes the note from the board for all connected users.

If a user saves changes to a note while another user simultaneously saves different changes to the same note, the most recent save wins — the board reflects whichever write arrived last.

If one user deletes a note while another user is editing or saving that same note, the outcome is determined by which action arrived at the server last. A delete that arrives last removes the note; a save that arrives last keeps the note with the saved content.

## Delete All

A **Delete All** button is permanently visible on the board (not inside any individual note). Clicking it removes every note from the board for all connected users at once.

## Live User Count

A visible element on the page displays how many users are currently connected to the board. This count updates in real time as users open or close the page — no reload required.

## Persistence

All notes (their text and position on the board) are stored in MySQL. Notes survive page reloads, backend restarts, and new user connections. A new visitor always sees the complete current state of the board.

## Page Structure

The app is a **single page at `/`**. There are no other frontend routes.

## `data-testid` Reference

Every interactive and observable element must carry the exact `data-testid` attribute listed below.

### Board

- `board` — the main board surface (the clickable area where notes live)
- `user-count` — element displaying the number of currently connected users
- `delete-all-btn` — the button that clears all notes from the board

### Notes

Each note has a unique `id` assigned by the backend. Use that id in the following patterns:

- `note-{id}` — the note card container
- `note-text-{id}` — the text field inside the note (editable when the note is active)
- `note-save-{id}` — the Save button for a note (only visible after the user has made changes to the text)
- `note-delete-{id}` — the Delete button for a note (visible when the note is active)
