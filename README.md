# VueSearch Application

This application allows users to manage email domains by grouping them into "Available" and "Selected" lists. Users can search for domains, move them between lists, and expand/collapse domain details.

## Key Features
1. **Search Functionality**:
   - Users can search for domains using the search bar.
   - Suggestions are dynamically displayed based on the search query.

2. **Collapsible Sections**:
   - Domains are displayed in collapsible sections.
   - Clicking on a domain header toggles its expanded/collapsed state.

3. **Move Between Lists**:
   - Users can move domains from the "Available" list to the "Selected" list using the `+` button.
   - Users can move domains back to the "Available" list using the `-` button.

---

## Code Explanation

### Single Array for Emails
The application uses a single array of objects (`emails`) to manage all email data. Each object contains:
- `email`: The email address.
- `isSelected`: A boolean indicating whether the email belongs to the "Selected" list.

This simplifies the logic by avoiding separate arrays for "Available" and "Selected" lists.

---

### Dynamic Filtering
The filtering logic is handled using computed properties:
- **`availableEmails`**: Filters emails where `isSelected` is `false`.
- **`selectedEmails`**: Filters emails where `isSelected` is `true`.

---

### Domain Suggestions
The `suggestions` computed property dynamically generates a list of unique domains based on the search query. It ensures that only domains matching the query are displayed in the suggestions dropdown.

---

### Collapsible State
The `collapsed` object tracks whether each domain is expanded or collapsed. Clicking on a domain header toggles its state.

---

### Toggle Selection
The `toggleSelection(domain, isSelected)` function updates the `isSelected` property for all emails in a given domain. This allows seamless movement of domains between the "Available" and "Selected" lists.

---

### Styling
- Buttons (`+` and `-`) have fixed dimensions (`32px x 32px`) for consistency.
- The `+` button is blue, and the `-` button is red for visual distinction.

---

## How to Use
1. Enter a search query in the search bar to filter domains.
2. Click the `+` button to move a domain to the "Selected" list.
3. Click the `-` button to move a domain back to the "Available" list.
4. Click on a domain header to expand or collapse its details.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Type Support for `.vue` Imports in TS

TypeScript cannot handle type information for `.vue` imports by default, so we replace the `tsc` CLI with `vue-tsc` for type checking. In editors, we need [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) to make the TypeScript language service aware of `.vue` types.

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Type-Check, Compile and Minify for Production

```sh
npm run build
```

### Run Unit Tests with [Vitest](https://vitest.dev/)

```sh
npm run test:unit
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```
