# Todo App

A simple Todo application built with Vue.js, Vite and Tailwind that allows users to create, view, and manage todo items, with data persistence using local storage.

## Project Structure

### 1. `App.vue`
- Main component managing the layout and state.
- Initializes and manages the `todos` array, syncing with local storage.

### 2. `components/Greeting.vue`
- Greets the user and allows name input.
- Stores and retrieves the name from local storage.

### 3. `components/CreateForm.vue`
- Form for adding new todos with content and category selection.
- Validates input and updates the parent `todos` array.

### 4. `components/List.vue`
- Displays the list of todos.
- Allows marking todos as done and deleting them.

## Local Storage
Uses local storage to persist todos and user name across sessions.
