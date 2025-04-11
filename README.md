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

### Dynamic Filtering (`filteredGroupedEmails`)
The `filteredGroupedEmails` computed property dynamically filters the `groupedEmails` object based on the search query and selected suggestion.

- **Default Behavior**: If no search query or suggestion is selected, it returns all grouped emails.
- **Filtered Behavior**: If a suggestion is selected, it returns only the emails for the selected domain.

**Syntax**:
```javascript
{ [selectedSuggestion.value]: groupedEmails.value[selectedSuggestion.value] }
```
- `[selectedSuggestion.value]`: Dynamically sets the property name to the selected domain.
- `groupedEmails.value[selectedSuggestion.value]`: Retrieves the emails for the selected domain.

---

### Moving Between Lists
- **`moveToSelected(domain)`**: Moves a domain from the "Available" list to the "Selected" list.
- **`moveToAvailable(domain)`**: Moves a domain back to the "Available" list.

---

### Collapsible State
The `collapsed` object tracks whether each domain is expanded or collapsed. Clicking on a domain header toggles its state.

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
