# ThinkFlow AI

[English](./README.md) | 中文

ThinkFlow AI 是一个轻量、偏本地化（Local-first）的“从想法到结构化图谱”的工作台：输入一个核心想法，应用会把它扩展为可浏览的知识图谱（模块/子模块）。每个节点支持继续追问扩展、深挖详情，以及可选的配图生成。

说明：当前仓库的 npm 包名仍为 `housegpt`（历史原因），产品名称为 **ThinkFlow AI**。

## 亮点功能

- **想法扩展为图谱**：生成根节点并扩展出结构化子节点。
- **上下文追问扩展**：对选中节点追加需求（follow-up 输入）并基于路径上下文扩展。
- **深挖详情**：为节点生成更详细的解释/补充内容并在节点内展开展示。
- **节点配图（可重试）**：为单个节点生成/重新生成图片，并支持全屏预览。
- **导出 Markdown**：将当前图谱导出为 Markdown 大纲（包含深挖内容）。
- **本地配置**：API 模式与配置保存到 `localStorage`，无需额外后端。
- **中英双语**：内置英文/中文 UI，一键切换。

## 技术栈

- **Vue 3**（Composition API）
- **Vite 5**
- **TypeScript**
- **Tailwind CSS**
- **VueFlow**（`@vue-flow/core` + background/controls/minimap）
- **vue-i18n**
- **lucide-vue-next**（图标）

## 项目结构

你最常关注的文件：

- `src/App.vue`：应用壳层，组合布局与事件转发。
- `src/composables/useThinkFlow.ts`：核心业务逻辑（节点/边操作、API 调用、持久化）。
- `src/components/TopNav.vue`：顶部工具栏（布局、导出、总结、设置、语言切换）。
- `src/components/BottomBar.vue`：底部输入条（核心想法输入 + 执行）。
- `src/components/WindowNode.vue`：自定义节点 UI（扩展/深挖/配图/follow-up）。
- `src/components/SettingsModal.vue`：API 模式与自定义端点配置 UI。
- `src/i18n/index.ts`：i18n 初始化（默认英文；语言写入 `localStorage`）。

## 核心原理（高层）

应用围绕一个 composable 构建：

- `useThinkFlow` 统一持有：节点/边数据、UI 状态、网络请求、错误处理、导出能力。
- 组件尽量“轻”：负责渲染与事件转发（expand、deep-dive、image、export 等）。
- 画布由 VueFlow 渲染，节点使用自定义类型 `window`。
- 配置持久化到 `localStorage`：
  - 语言：`language`
  - API 模式与 chat/image 的 baseUrl/model/apiKey

## 快速开始

### 环境要求

- Node.js **18+**（Vite 5 要求）
- npm（也可以用 pnpm/yarn；示例使用 npm）

### 安装依赖

```bash
npm install
```

### 启动开发环境

```bash
npm run dev
```

打开 Vite 输出的地址（通常是 `http://localhost:5173`）。

### 构建

```bash
npm run build
```

### 预览构建产物

```bash
npm run preview
```

## 配置说明

### 自定义模式：自带端点（BYO Endpoint）

在界面里打开 **Settings**：

- 切换到 **Custom**
- 配置 **文本生成（chat completion）**
  - `baseUrl`（POST 接口地址）
  - `model`
  - `apiKey`
- 配置 **图片生成**
  - `baseUrl`（POST 接口地址）
  - `model`
  - `apiKey`

这些字段会自动保存到 `localStorage`。

## 功能使用说明

### 1）从核心想法生成图谱

1. 在底部输入框输入核心想法
2. 回车或点击 **Execute**
3. 生成根节点并自动扩展出子节点

### 2）对某个节点继续扩展（Follow-Up）

1. 在节点的 follow-up 输入框填写“追加需求”
2. 回车触发扩展
3. 扩展时会携带“从根到当前节点”的上下文路径，保证更贴合当前分支

### 3）深挖节点详情

点击节点上的 **Deep Dive**：

- 生成更长的解释/拓展内容
- 内容会写回节点并可重复打开（不必每次都重新请求）

### 4）节点配图与预览

- 点击节点上的 **IMG** 生成配图
- 点击图片打开全屏预览
- 支持在预览层或节点上进行重新生成

### 5）导出 Markdown

顶部工具栏点击 **Export**：

- 根节点作为一级标题
- 子节点按缩进列表输出
- 深挖内容作为引用块输出到对应节点下方

## 第三方集成与插件

- **Microsoft Clarity**：在 `index.html` 中引入，用于分析与统计。
- **VueFlow 插件**：
  - Background（Dots/Lines）
  - Controls（缩放/适配）
  - MiniMap（总览）

## 国际化（i18n）

- 默认语言：**英文**
- 已支持：`en`、`zh`
- 语言选择写入 `localStorage` 的 `language`

新增语言建议步骤：

1. 在 `src/i18n/locales/` 添加对应 JSON
2. 在 `src/i18n/index.ts` 注册 messages
3. 在 UI 中扩展语言切换逻辑（目前为 EN/ZH 双向切换）

## 常见问题排查

- **CORS / Failed to fetch**：使用自定义端点时，确保支持浏览器跨域请求并正确返回 CORS 响应头。
- **401/403**：检查 API Key 是否正确，以及是否使用 `Authorization: Bearer ...`。
- **429**：触发限流，稍后重试。

## Scripts

- `npm run dev`：启动开发服务器
- `npm run build`：构建生产包
- `npm run preview`：本地预览构建产物
