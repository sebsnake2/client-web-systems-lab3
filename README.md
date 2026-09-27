# Client Web Systems — Laboratory Work 3

## Vue User List

A Vue 3 application developed with TypeScript using the Composition API and Single File Components.

The project demonstrates Vue components, directives, reactive state, filtering, sorting and dynamic rendering.

## Technologies

- Vue 3
- TypeScript
- Vite
- Composition API
- Single File Components
- ESLint
- Oxlint

## Features

- User profile component
- List of 10 users
- Local user images
- Dynamic rendering with `v-for`
- Conditional rendering with `v-if`
- Details visibility with `v-show`
- Dynamic class binding with `v-bind`
- Gender filtering
- Age 18+ filtering
- Sorting by name
- Sorting by age
- Combined filtering and sorting
- Reset filters and sorting
- Empty list state
- Hobbies rendering
- Responsive user cards

## Age-based Styling

User cards receive a dynamic class depending on age:

- under 18 — `minor`
- 18–30 — `young`
- 31–50 — `adult`
- over 50 — `senior`

## Project Structure

```text
src/
├── components/
│   └── Users.vue
├── data/
│   └── user.json
├── assets/
│   └── main.css
├── App.vue
└── main.ts

public/
├── users/
│   └── user images
└── tutorial/
    ├── step-1.png
    ├── ...
    └── step-15.png
```

Vue Tutorial
All 15 steps of the official Vue tutorial were completed.
Screenshots of the completed tasks are stored in:
public/tutorial/

Project Setup
Install dependencies:
npm install

Run the development server:
npm run dev

Run linting:
npm run lint

Create a production build:
npm run build

Main Vue Concepts Used
The application demonstrates:

- ref
- computed
- v-bind
- v-on
- v-model
- v-if
- v-else
- v-show
- v-for
- components
- props
- emits
- slots
- lifecycle hooks
- watchers
