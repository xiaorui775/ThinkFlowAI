# ThinkFlow AI

English | [中文](./README.zh-CN.md)

ThinkFlow AI is a lightweight, local-first idea-to-structure workspace. You type a core idea, and the app expands it into a navigable knowledge graph (modules/submodules). Each node supports follow-up expansion, deep-dive details, and optional image generation.

This repository currently uses the package name `thinkflow` (historical) while the product name is **ThinkFlow AI**.

## Highlights

- **Idea expansion to graph**: generate a root node and expand into structured child nodes.
- **Context-aware follow-up**: expand a selected node with an additional requirement (follow-up input).
- **Deep dive**: generate detailed explanations for a node and show them inline.
- **Image generation (per node)**: generate/regenerate a visual for a node and preview it fullscreen.
- **Export to Markdown**: export the current graph as a Markdown outline (including deep-dive content).
- **Local-first settings**: API mode and endpoints are stored in `localStorage` (no backend needed).
- **Bilingual UI**: built-in English/Chinese UI with a one-click toggle.

## Tech Stack

- **Vue 3** (Composition API)
- **Vite 5**
- **TypeScript**
- **Tailwind CSS**
- **VueFlow** (`@vue-flow/core`, `background`, `controls`, `minimap`)
- **vue-i18n**
- **lucide-vue-next** (icons)

## Project Structure

Key files you will touch most often:

- `src/App.vue`: app shell, composes the layout and wires UI to business actions.
- `src/composables/useThinkFlow.ts`: core business logic (graph operations, API calls, persistence).
- `src/components/TopNav.vue`: top toolbar (layout, export, summary, settings, language toggle).
- `src/components/BottomBar.vue`: bottom input bar (core idea input + execute).
- `src/components/WindowNode.vue`: custom VueFlow node UI (expand/deep-dive/image/follow-up).
- `src/components/SettingsModal.vue`: API mode and custom endpoints configuration UI.
- `src/i18n/index.ts`: i18n bootstrap (default English; persists language in `localStorage`).

## How It Works (High-Level)

ThinkFlow AI is built around a single composable:

- `useThinkFlow` owns all reactive state for nodes/edges and UI flags.
- UI components are “thin”: they render and forward events (expand, deep-dive, image, export).
- The graph is rendered with VueFlow using a custom node type (`window`).
- Settings are stored in `localStorage`:
    - language (`language`)
    - API mode + chat/image endpoints/models/keys

## Getting Started

### Prerequisites

- Node.js **18+** (required by Vite 5)
- npm (or pnpm/yarn; examples use npm)

### Install

```bash
npm install
```

### Run Dev Server

```bash
npm run dev
```

Then open the URL printed by Vite (usually `http://localhost:5173`).

### Build

```bash
npm run build
```

### Preview Production Build

```bash
npm run preview
```

## Configuration

### Custom Mode (Bring Your Own Endpoint)

Open **Settings** in the UI:

- Switch to **Custom**
- Configure **Text Generation** (chat completion)
    - `baseUrl` (POST endpoint)
    - `model`
    - `apiKey`
- Configure **Image Generation**
    - `baseUrl` (POST endpoint)
    - `model`
    - `apiKey`

These fields are saved into `localStorage` automatically.

## Features Guide

### 1) Expand From a Core Idea

1. Type a core idea in the bottom input.
2. Press Enter or click **Execute**.
3. A root node appears and the app generates child nodes.

### 2) Expand a Specific Node (Follow-Up)

1. Click into a node’s follow-up input.
2. Type an additional requirement.
3. Press Enter to expand using the context path (root → current node).

### 3) Deep Dive a Node

Click **Deep Dive** on a node to generate a detailed explanation. The content will be stored on the node and can be reopened without re-requesting (unless you expand further).

### 4) Generate/Preview Node Image

- Click **IMG** to generate an image for a node.
- Click the image to open fullscreen preview.
- Use the regenerate action to request a new image.

### 5) Export Markdown

Use **Export** in the top toolbar:

- Root becomes the Markdown title
- Child nodes become an indented list
- Deep-dive text is exported as blockquote content under the node

## Third-Party Integrations

- **Microsoft Clarity**: included in `index.html` for analytics.
- **VueFlow plugins**:
    - Background (Dots/Lines)
    - Controls (zoom/fit)
    - MiniMap (overview)

## Internationalization (i18n)

- Default locale: **English**
- Supported: `en`, `zh`
- Language choice is stored in `localStorage` key `language`

To add a new language:

1. Add a JSON file in `src/i18n/locales/`
2. Register it in `src/i18n/index.ts`
3. Add a toggle option in the UI (currently a simple EN/ZH switch)

## Common Troubleshooting

- **CORS / Failed to fetch**: if you use a custom endpoint, ensure it allows browser requests and supports proper CORS headers.
- **401/403**: verify the API key and `Authorization: Bearer ...` format.
- **429**: rate limited by the provider; retry later.

## Scripts

- `npm run dev`: start Vite dev server
- `npm run build`: production build
- `npm run preview`: preview the build output locally
