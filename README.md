# 🔍 PerCLI — AI-Powered Search Assistant for your Terminal

<p align="center">
  <strong>A command-line AI assistant that combines Google Gemini with Tavily Search to provide intelligent, source-backed answers directly from your terminal.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/node-%3E%3D18-brightgreen?style=flat-square&logo=node.js" alt="Node.js">
  <img src="https://img.shields.io/badge/license-ISC-blue?style=flat-square" alt="License">
  <img src="https://img.shields.io/badge/version-1.0.0-orange?style=flat-square" alt="Version">
  <img src="https://img.shields.io/badge/AI-Gemini%202.5%20Flash-blueviolet?style=flat-square&logo=google" alt="Gemini">
  <img src="https://img.shields.io/badge/Search-Tavily-teal?style=flat-square" alt="Tavily">
  <img src="https://img.shields.io/badge/built%20with-Genkit-red?style=flat-square&logo=firebase" alt="Genkit">
</p>

---

> 🌐 **[Versión en español](./README_ES.md)**

---

## ✨ Features

- 🤖 **AI-Powered Answers** — Uses Google Gemini 2.5 Flash for natural language understanding and generation
- 🔎 **Real-Time Web Search** — Leverages Tavily API with advanced search depth to ground responses with up-to-date web data
- 💬 **Interactive Chat Mode** — Conversational interface with session-based chat history maintained in memory
- 🛠️ **Genkit Tool System** — Search is exposed as a Genkit tool, allowing the AI to autonomously decide when to search the web
- 📑 **Source Citations** — Responses include numbered source citations and a dedicated Sources section with URLs
- 🎨 **Beautiful Terminal UI** — Colored output, spinners, and formatted responses using Chalk and Ora
- 🔧 **Genkit Dev Tools** — Built on [Firebase Genkit](https://firebase.google.com/docs/genkit) for observability, tracing, and debugging

## 📋 Prerequisites

- [Node.js](https://nodejs.org/) **v18 or higher**
- A [Tavily API Key](https://tavily.com) (for web search)
- A [Google AI API Key](https://aistudio.google.com/app/apikey) (for Gemini model)

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/juanan09/percli.git
cd percli
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Copy the example file and add your API keys:

```bash
cp .env.example .env
```

Edit `.env` with your keys:

```env
# Tavily API Key - Get it from https://tavily.com
TAVILY_API_KEY=your_tavily_api_key_here

# Google AI API Key - Get it from https://aistudio.google.com/app/apikey
GOOGLE_API_KEY=your_google_api_key_here
```

### 4. Run the application

```bash
# Production mode
npm start

# Development mode (with Genkit Dev UI for tracing and debugging)
npm run dev
```

## 🎮 Usage

Once running, you'll see an interactive prompt:

```
╔════════════════════════════════════════════════════╗
║   Welcome to Percli CLI - Interactive Mode     ║
╚════════════════════════════════════════════════════╝
Type your questions and get AI-powered answers with sources!
Chat history is maintained during this session.
Commands: exit, quit, or press Ctrl+C to leave

💬 Ask a question (or type "exit" to quit):
```

Simply type your question and press Enter. The AI will:

1. Analyze your query
2. Automatically search the web if current information is needed
3. Synthesize information from multiple sources
4. Provide a comprehensive, well-structured answer with source citations

**Commands:**

| Command         | Description          |
| --------------- | -------------------- |
| `exit` / `quit` | Exit the application |
| `Ctrl+C`        | Force quit           |

> **💡 Tip:** Chat history is maintained during your session, so the AI remembers previous questions and can build on prior context.

## 🏗️ Architecture

PerCLI is built on a **tool-augmented agent** architecture using Firebase Genkit:

```
┌──────────────────────────────────────────────────────────┐
│                    index.js (CLI Loop)                    │
│  ┌─────────────┐  ┌──────────┐  ┌─────────────────────┐ │
│  │   readline   │  │  chalk   │  │        ora          │ │
│  │  (user I/O)  │  │ (colors) │  │    (spinners)       │ │
│  └──────┬───────┘  └──────────┘  └─────────────────────┘ │
│         │                                                 │
│         ▼                                                 │
│  ┌─────────────────────────────────────────────────────┐  │
│  │              agent.js (Chat Agent)                  │  │
│  │  ┌──────────────────┐  ┌─────────────────────────┐  │  │
│  │  │  Genkit Prompt   │  │     searchWeb Tool     │  │  │
│  │  │  (system prompt  │  │  (AI decides when to   │  │  │
│  │  │   + model cfg)   │  │   invoke search)       │  │  │
│  │  └──────────────────┘  └────────────┬────────────┘  │  │
│  └─────────────────────────────────────┤───────────────┘  │
│                                        │                  │
│  ┌─────────────────────────────────────▼───────────────┐  │
│  │            search.js (Tavily Integration)           │  │
│  │  ┌──────────────────────────────────────────────┐   │  │
│  │  │  Advanced search · Max N results · Answer    │   │  │
│  │  └──────────────────────────────────────────────┘   │  │
│  └─────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
```

### How It Works

1. **`index.js`** — Entry point. Sets up the Genkit AI instance, Tavily client, and readline-based interactive loop. Validates environment variables and manages the CLI lifecycle.
2. **`src/agent.js`** — Defines the AI chat agent. Creates a Genkit prompt with a system instruction that tells the AI how to respond, and registers a `searchWeb` tool that the AI can call autonomously. Uses Zod schemas for input/output validation.
3. **`src/search.js`** — Wraps the Tavily API client. Performs advanced web searches with configurable result limits and returns structured results (title, URL, content).

## 📁 Project Structure

```
percli/
├── index.js              # Entry point — interactive CLI loop & initialization
├── src/
│   ├── agent.js          # Chat agent config — prompt, model, and tool registration
│   └── search.js         # Tavily web search wrapper with advanced search depth
├── .env.example          # Environment variables template
├── .gitignore            # Git ignore rules
├── package.json          # Dependencies, scripts, and project metadata
├── README.md             # This file (English)
└── README_ES.md          # Documentation in Spanish
```

## 📦 Dependencies

### Production

| Package                                                                          | Version | Description                                                    |
| -------------------------------------------------------------------------------- | ------- | -------------------------------------------------------------- |
| [genkit](https://www.npmjs.com/package/genkit)                                   | ^1.29.0 | Firebase Genkit framework for building AI-powered applications |
| [@genkit-ai/google-genai](https://www.npmjs.com/package/@genkit-ai/google-genai) | ^1.29.0 | Google Gemini plugin for Genkit                                |
| [@genkit-ai/googleai](https://www.npmjs.com/package/@genkit-ai/googleai)         | ^1.28.0 | Google AI integration for Genkit                               |
| [@tavily/core](https://www.npmjs.com/package/@tavily/core)                       | 0.7.2   | Tavily search API client for real-time web search              |
| [chalk](https://www.npmjs.com/package/chalk)                                     | ^5.6.2  | Terminal string styling with colors and formatting             |
| [ora](https://www.npmjs.com/package/ora)                                         | ^9.3.0  | Elegant terminal spinners for loading states                   |
| [dotenv](https://www.npmjs.com/package/dotenv)                                   | ^17.3.1 | Loads environment variables from `.env` files                  |

### Development

| Package                                                | Version | Description                                   |
| ------------------------------------------------------ | ------- | --------------------------------------------- |
| [genkit-cli](https://www.npmjs.com/package/genkit-cli) | ^1.29.0 | Genkit CLI for dev UI, tracing, and debugging |

## 📜 Scripts

| Script  | Command       | Description                                                                             |
| ------- | ------------- | --------------------------------------------------------------------------------------- |
| `start` | `npm start`   | Runs the CLI in production mode (`node index.js`)                                       |
| `dev`   | `npm run dev` | Runs with Genkit Dev UI for tracing and debugging (`npx genkit start -- node index.js`) |
| `test`  | `npm test`    | Placeholder for tests (not yet implemented)                                             |

## 🔧 Configuration

All configuration is done through environment variables in the `.env` file:

| Variable         | Required | Description                                                                               |
| ---------------- | -------- | ----------------------------------------------------------------------------------------- |
| `TAVILY_API_KEY` | ✅        | API key for Tavily web search — get it at [tavily.com](https://tavily.com)                |
| `GOOGLE_API_KEY` | ✅        | API key for Google Gemini — get it at [AI Studio](https://aistudio.google.com/app/apikey) |

## 🧠 AI Model

PerCLI uses **Google Gemini 2.5 Flash** as its underlying language model, configured through Genkit. The model is instructed to:

- Provide comprehensive, factual answers based on web search results
- Synthesize information from multiple sources
- Use numbered citations `[1]`, `[2]`, etc. to reference sources
- Include a "Sources" section at the end of each response with reference URLs
- Use markdown formatting for better readability
- Autonomously decide when to use the web search tool

## 🛡️ Error Handling

PerCLI includes robust error handling:

- **Missing API keys** — Clear error messages with actionable tips if `TAVILY_API_KEY` or `GOOGLE_API_KEY` are not set
- **Search failures** — Tavily search errors are caught and surfaced with descriptive messages
- **Runtime errors** — Per-query errors are caught without crashing the session, allowing continued interaction
- **Graceful shutdown** — Clean exit with farewell message on `exit`, `quit`, or `Ctrl+C`

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the **ISC License**.

---

<p align="center">
  Made with ❤️ using <a href="https://firebase.google.com/docs/genkit">Genkit</a>, <a href="https://ai.google.dev/">Google Gemini</a>, and <a href="https://tavily.com">Tavily</a>
</p>
