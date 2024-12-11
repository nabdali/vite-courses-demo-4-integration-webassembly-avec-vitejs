# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh

## Steps to Reproduce

If you'd like to recreate this repository, you can follow these instructions:

1. Install wasm-pack.

   ```sh
   cargo install wasm-pack
   ```

1. Initialize a new wasm-pack project.

   ```sh
   wasm-pack new vite-rust-wasm
   ```

1. Initialize a Vite project in the same folder.

   ```sh
   cd vite-rust-wasm
   npm init vite .
   ```

   Make sure to ignore existing files and continue.

   You will also have the opportunity to choose which frontend framework, and whether to use TypeScript or JavaScript. For this repo, we used Svelte.

1. Install additional Node dev dependencies.

   ```sh
   npm install -D vite-plugin-wasm vite-plugin-top-level-await
   ```

1. Update `vite.config.ts`.

   ```typescript
   import { defineConfig } from 'vite'
    import react from '@vitejs/plugin-react'
    import wasm from "vite-plugin-wasm";
    import topLevelAwait from "vite-plugin-top-level-await";

    // https://vite.dev/config/
    export default defineConfig({
    plugins: [react(), wasm(), topLevelAwait()],
    })
   ```

1. Update `src/lib.rs`.

   ```rust
   mod utils;

   use wasm_bindgen::prelude::*;

   #[wasm_bindgen]
   pub fn greet(name: &str) -> String {
       format!("Hello from Rust, {}!", name)
   }
   ```

1. Update `src/App.svelte`.

   ```javascript
    import { useState } from 'react'
    import reactLogo from './assets/react.svg'
    import viteLogo from '/vite.svg'
    import './App.css'
    import { greet } from "../pkg/vite_rust_wasm";

    function App() {
    const [count, setCount] = useState(0)

    return (
        <>
        <div>
            <a href="https://vite.dev" target="_blank">
            <img src={viteLogo} className="logo" alt="Vite logo" />
            </a>
            <a href="https://react.dev" target="_blank">
            <img src={reactLogo} className="logo react" alt="React logo" />
            </a>
        </div>
        <h1>Vite + Svelte + WebAssembly</h1>
        <h2>{greet("JUNIA XP students!")}</h2>
        </>
    )
    }

    export default App
   ```

1. Build the WebAssembly.

   ```sh
   wasm-pack build
   ```

1. Run the dev server.

   ```sh
   npm run dev
   ```

1. Navigate to http://localhost:5173/
