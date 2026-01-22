# ThinkFlow AI

[中文](./README.md) | English

**ThinkFlow AI** is a next-generation, local-first, AI-driven mind mapping tool built with Vue 3 and VueFlow. More than just a drawing app, it serves as a "brain augmentor" that thinks with you. By combining the divergent capabilities of LLMs (Large Language Models) with structured visualization, ThinkFlow AI rapidly transforms fuzzy ideas into clear, deep knowledge systems.

This project was built through Vibe Coding (requirement-driven, AI-assisted iteration).

---

## 🔗 Live Demo

Experience it now: [https://thinkflow-ai.lz-t.top/](https://thinkflow-ai.lz-t.top/)

---

## 🌟 Why ThinkFlow AI?

Traditional mind mapping tools often require manual entry of every branch, which can become a bottleneck during a burst of inspiration. ThinkFlow AI redefines this process:

1.  **AI-Driven Automated Divergence**: Enter a core keyword, and the AI automatically expands outward based on logical chains, helping you break through "blank page anxiety."
2.  **Context-Aware Deep Dialogue**: Every node carries its full path context within the thinking tree. This means the AI understands _why_ you moved from A to B, generating more precise follow-up suggestions.
3.  **Multi-Dimensional Sensory Presentation**: Gain textual knowledge through deep "Answers," visual imagery through AI Image Generation, and macro insights through "Session Summaries."
4.  **Ultimate Privacy & Freedom**: Adopts a local-first architecture. Configurations are stored locally in your browser. Supports any OpenAI-compatible interface with no platform lock-in.

---

## 🚀 Core Functional Architecture

### 1. Intelligent Expansion System

- **Core Idea Activation**: Generate the foundation of your thinking tree with one click.
- **Path Contextual Follow-up**: Perform follow-ups on nodes; the AI reasons using the full logical path from the root to the current node.
- **Node Management**: Supports subtree collapsing for large-scale graphs to keep the workspace clean.

### 2. Deep Dive & Summarization

- **Deep Answer (Deep Dive)**: Generates 300-500 words of professional analysis for specific concepts, supporting rich Markdown formatting.
- **Visual Generation (Image Gen)**: Utilizes CogView or DALL-E to generate photorealistic visuals for nodes, reinforcing visual memory.
- **Global Summary**: Automatically analyzes the entire graph's logic to extract core insights and conclusions.

### 3. Canvas Interaction & Layout

- **Smart Tree Layout**: Built-in algorithm based on dynamic subtree height calculations to solve overlap issues after node expansion.
- **Linked Dragging**: When a parent node moves, child nodes move synchronously to maintain relative positions.
- **Versatile Export**: Perfectly supports exporting to structured Markdown, preserving all deep answer content.

---

## 🛠️ Tech Stack

| Dimension                | Technology Choice                                      |
| :----------------------- | :----------------------------------------------------- |
| **Frontend Framework**   | Vue 3 (Composition API)                                |
| **Build Tool**           | Vite 5 + TypeScript                                    |
| **Graph Engine**         | @vue-flow/core (High performance, highly customizable) |
| **UI/Styling**           | Tailwind CSS + Lucide Icons                            |
| **Internationalization** | Vue-I18n (Seamless English/Chinese switching)          |
| **Markdown**             | Markdown-it (Node content rendering)                   |

---

## 📂 Source Structure

```text
src/
├── components/             # UI Component Layer
│   ├── WindowNode.vue      # Logic Carrier: Custom node with Image/Answer/Follow-up capabilities
│   └── ...
├── services/               # Base Service Layer
│   └── config.ts           # Core Config: API endpoints, model names, and global API Key
├── composables/            # Domain Logic Layer
│   └── useThinkFlow.ts     # Business Heart: State management, API scheduling, layout algorithm
├── i18n/                   # Language Assets
│   └── locales/            # zh.json / en.json translation files
├── App.vue                 # App Skeleton: VueFlow container config and assembly
└── main.ts                 # Entry Point
```

---

## 🚀 Quick Start

```bash
# 1. Clone the repo
git clone https://github.com/your-repo/ThinkFlowAI.git

# 2. Install dependencies
npm install

# 3. Start development
npm run dev
```

---

## ⚙️ API Service Description (Public Demo)

To provide an out-of-the-box experience, this project includes a default set of demo endpoints.

- **Service Proxying**: Default requests are proxied and controlled via **Cloudflare Workers**.
- **Model Support**: Connected to Zhipu Bigmodel (glm-4-flash / cogview-3-flash) using free tiers.
- **Limitations**: As these are public demo endpoints, you may encounter rate limits or exhausted quotas.

---

## ⚙️ Deployment & Configuration (Self-Hosting Guide)

If you wish to self-host this project (e.g., for internal use or sharing with friends), we **recommend** following these steps to modify the backend service configuration to avoid rate limits on public demo endpoints.

### 1. Modify Default API Config

Open `src/services/config.ts` and modify the `DEFAULT_CONFIG` constant:

```typescript
// src/services/config.ts
export const DEFAULT_CONFIG = {
    chat: {
        // Your Chat API endpoint (OpenAI compatible)
        // If you encounter CORS issues, use it with vite.config.ts proxy (e.g., '/api/chat')
        baseUrl: 'https://your-private-api.com/v1/chat/completions',
        model: 'gpt-4o',
        apiKey: '' // (Optional) Recommend filling in the default Key for private deployment
    },
    image: {
        baseUrl: 'https://your-private-api.com/v1/images/generations',
        model: 'dall-e-3',
        apiKey: ''
    }
}
```

### 2. Handling CORS Issues

If your API does not support CORS, you can configure a proxy in `vite.config.ts`:

```typescript
// vite.config.ts
export default defineConfig({
    server: {
        proxy: {
            '/api': {
                target: 'https://your-private-api.com/v1',
                changeOrigin: true,
                rewrite: path => path.replace(/^\/api/, '')
            }
        }
    }
})
```

After configuring the proxy, update `baseUrl` in `config.ts` to a relative path (e.g., `/api/chat/completions`).

### 3. Build for Production

```bash
# Execute build
npm run build
```

---
